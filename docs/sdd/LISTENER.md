← Back to [MAIN](MAIN.md)

# Listener

- **Module:** Listener
- **Module code:** LSN
- **Version:** v0.2
- **Status:** Draft
- **Last updated:** 2026-10-06

> **Draft.** Specification is not finished. Decided items are kept as prose under "Decided so far" until requirements (`REQ-LSN-NNN`) and acceptance criteria are written; everything under "Open points" needs the user's decision before approval (`AGENTS.md` §6.3).

## Description

Real-time ingestion of the Telegram channel of the Empresa Eléctrica de Holguín, which constantly publishes which circuits are affected by blackouts and how many hours of outage each has accumulated, and parsing of its messages into circuit states and the provincial situation for [CIRCUITS.md](CIRCUITS.md).

## Decided so far

### Connection

- Channel: **https://t.me/elecholguin**.
- Telethon connects to Telegram with the user's **personal** Telegram account.
- The Telethon **session** is generated on the user's own machine (login code / 2FA there) and stored as a **secret of the hosting environment**, together with the Telegram API credentials. It is **never** stored in the repository.
- The listener runs inside the backend worker process ([ARCHITECTURE.md](ARCHITECTURE.md)).

### Session safety (single instance)

Only one listener may be connected to Telegram with the session at any time.

- The process that runs the listener is deployed with a **single replica**.
- In addition, on start the listener takes a **lock in the database** before connecting to Telegram. If another **live** instance holds the lock, the listener **waits without connecting to Telegram** until the lock is released, then takes it and connects.
- The lock is released when the holding instance stops, including when it dies or loses its database connection, so a crashed instance never blocks the next one forever.

### Message kinds

- **Status message:** recognised by the `ACTUALIZACIÓN` header. It is parsed as described below.
- **Daily situation message:** a "Nota informativa" (about the national grid, SEN) or a provincial daily situation message (`Holguín, <date> … Situación del SEN …`). It is not parsed: its raw text and publish time are passed on as the latest daily situation ([CIRCUITS.md](CIRCUITS.md)). It triggers no notification.
- **Any other message** (e.g. per-circuit notices with images) is ignored.

### Status message format

A status message (see the [reference sample](#reference-sample)) has three parts, one per line or block:

1. **Header line:** `🚨ACTUALIZACIÓN <day> de <month> a las <h:mm> <AM|PM>🚨`, e.g. `🚨ACTUALIZACIÓN 4 de Octubre a las 7:27 PM🚨`. The date has no year.
2. **Summary paragraph** (starts with `👉`): states the served demand of the province in MW (in the sample also the MW served to "el Níquel"), the maximum outage time (`H:MM horas`) and the cut-off time (`con cierre a las <h:mm> <AM|PM>`).
3. **Circuit lines**, one per affected entry, in the form `📌<name><separator> <H:MM> <Horas|Minutos> [cause]`:
   - The **separator** varies (`-`, `.-`, ` - `, `- `). The separator is the last `-` or `.-` immediately before the duration; everything before it is the name. Hyphens inside the name (`Nipe-Cueto`, `Canela-Guardalavaca 1`, `Nicaro - Fábrica 2`, `Nicaro- Cabonico`) belong to the name.
   - The **duration** is always `H:MM` (hours may exceed 24, e.g. `27:50`). The unit word `Horas` or `Minutos` does not change the reading: `0:29 Minutos` is 0 hours 29 minutes.
   - The optional **cause** follows the duration, with or without parentheses (`(Avería)`, `Emergencia`).
   - Leading/trailing whitespace on a line is ignored.

### Interpretation rules

- **State timestamp:** the date and time in the **header** is the timestamp of the state. If the header **date/time** cannot be read (the header itself is still recognised), the Telegram publish time is used instead; the rest of the message must still parse fully.
- **Year and time zone:** the header year is taken from the Telegram publish date. All times (header, cut-off) are interpreted in **Cuba local time**.
- **Parenthesised parts (ramals):** a name with a parenthesised part affects the **base circuit** (the text before the parentheses), and the parenthesised text is kept as a detail of that circuit's state. With a `Ramal <name>` part, e.g. `Cto Banes 2 (Ramal Tanganica)`, the detail is the ramal name (`Tanganica`); without it, e.g. `Cto 4 de Moa(Los Mangos)` or `Cto 6 de Moa(Rolo)`, the parenthesised text (`Los Mangos`, `Rolo`) is the detail and the base circuit is `Cto 4 de Moa` / `Cto 6 de Moa`.
- **Combined entries:** a name of the form `<base> <a> y <b>`, e.g. `Zarzal 1 y 2`, is split into separate circuits (`Zarzal 1`, `Zarzal 2`), each with the same duration and cause. Other combined forms (e.g. `Zarzal 1, 2 y 3`, `Zarzal 1 al 3`) are **not understood** until they are seen in the channel and specified.
- **Cause:** **any** trailing text after the duration is the cause. It is stored with the circuit's state (shown per [CIRCUITS.md](CIRCUITS.md) and [NOTIFICATIONS.md](NOTIFICATIONS.md)).
- **Same circuit more than once:** if, after ramal resolution, splitting and normalisation, a circuit appears several times in one message, its state takes the **longest duration** (with the cause of that entry), and **all** ramal/parenthesised details are kept.
- **Provincial situation:** the served demand (MW), the MW served to "el Níquel", the maximum outage time and the cut-off time from the summary paragraph are extracted as the current provincial situation ([CIRCUITS.md](CIRCUITS.md)). The parser only looks for these values and ignores the rest of the prose; if the served demand, the maximum outage time or the cut-off time is missing, the paragraph is not understood (the whole message is discarded). The Níquel figure is **optional**: without it the message is still understood and the Níquel line is simply not shown.
- **Circuit identity:** names are matched to the catalog by the normalised identity defined in [CIRCUITS.md](CIRCUITS.md). Circuits and lines (e.g. `Banes-Antilla`) are treated identically.
- **No circuit lines:** a status message with no circuit lines is **discarded**.
- **All or nothing:** if a status message contains **any** line the parser cannot understand, the **whole message is discarded**: the current state is left untouched, and the message is **logged only** for review (no alert).
- **Out-of-order messages:** a status message whose state timestamp is older than the current state is **ignored**.
- **Edited status message:** if the channel edits an already processed status message and it is the **current** one (the latest applied), it is re-parsed and applied again, and only the resulting changes are notified. If it is an **older** one, only the history is corrected ([HISTORY.md](HISTORY.md)); the current state and notifications are not affected. If an edit makes the current status message not understood or leaves it with no circuit lines, it is handled **as a deletion** (see below).
- **Deleted status message:** if the channel deletes the **current** status message, the state **reverts** to that of the previous applied status message, and the resulting changes are notified like any other state change. If an **older** one is deleted, it is only removed from the history.

### History, first start and catch-up

- Every processed status message is stored in the history ([HISTORY.md](HISTORY.md)), retained for the last **3 months**.
- **First start:** the listener imports the channel's past messages available within the retention window (3 months), in order, building state and history, **without notifying**.
- **Catch-up after downtime:** on reconnect, all missed status messages are processed **in order** for state and history, but only the changes of the **most recent** one generate notifications ([NOTIFICATIONS.md](NOTIFICATIONS.md)).

## Reference sample

A real status message from the channel, kept verbatim (including trailing spaces) as the reference for deriving parser tests.

```text
🚨ACTUALIZACIÓN 4 de Octubre a las 7:27 PM🚨
👉En estos momentos la provincia se mantiene con una demanda servida de 68 MW,de ellos 1 MW en el Níquel. Con un tiempo máximo de afectación de 20:53 horas, con cierre a las 7:20 PM. A continuación relacionamos los circuitos y líneas sin servicio eléctrico, iniciando por el de mayor tiempo de afectación.
📌Cto Uñas 1- 27:50 Horas (Avería)
📌Zarzal 1 y 2.- 25:33 Horas Emergencia
📌Aereopuerto 1- 20:53 Horas
📌Cto 17- 20:53 Horas 
📌Cto 21- 19:09 Horas 
📌Canela Pesquero 1- 18:57 Horas 
📌Cto 11- 18:50 Horas 
📌Sao Arriba- 18:44 Horas 
📌Cto Banes 5- 18:43 Horas 
📌Cto Banes 1- 18:43 Horas 
📌Cto Banes 4- 18:43 Horas 
📌Guerrita- 15:57 Horas 
📌Arroyo del Medio (Ramal El Cocal)- 15:57 Horas 
📌Iberia 1- 15:57 Horas 
📌Cto 13- 15:57 Horas 
📌Iberia 2- 15:57 Horas 
📌Juan Vicente- 15:57 Horas 
📌Guatemala- 15:57 Horas 
📌Urbano Noris 1- 15:57 Horas 
📌Urbano Noris 2- 15:57 Horas 
📌Cto La Caridad- 15:57 Horas 
📌La Naza- 15:47 Horas 
📌Ocujal- 15:37 Horas 
📌A. Pino- 14:13 Horas 
📌Cto Gibara 1- 12:57 Horas 
📌Cto Gibara 2- 12:57 Horas 
📌Cto Santa María- 12:57 Horas 
📌Aguas Claras- 12:56 Horas 
📌Piedra Blanca- 12:43 Horas 
📌Canela-Guardalavaca 1- 12:15 Horas
📌Cto 14- 10:30 Horas 
📌Cto 3- 10:30 Horas 
📌Holguín Maceo- 10:17 Horas 
📌Fray Benito- 7:38 Horas 
📌Nipe-Cueto(Ramal Cueto)- 6:29 Horas 
📌Cto Palmarito- 6:29 Horas 
📌Cto Tacamara- 6:29 Horas 
📌Holguín Mir- 6:26 Horas 
📌Tacajó 1- 5:31 Horas 
📌Tacajó 2- 5:31 Horas 
📌Cto Baguanos 2- 5:22 Horas 
📌Cto Baguanos 3 - 5:22 Horas 
📌Cto Nipe- 4:50 Horas 
📌Nicaro - Fábrica 2- 4:50 Horas 
📌Cto Lote Seco- 4:49 Horas 
📌Cto Pdo Nicaragua- 4:47 Horas 
📌Velasco 2- 4:46 Horas 
📌Cto Uñas 2- 4:18 Horas 
📌Cto 2- 4:05 Horas 
📌Guaro- 3:58 Horas 
📌Nicaro- Cabonico- 3:07 Horas 
📌Cto 22- 3:00 Horas 
📌La Sirena- 3:00 Horas 
📌Cto 15- 2:00 Horas 
📌Cto 16- 2:00 Horas 
📌Revolución- 1:54 Horas 
📌Mayarí 1- 1:54 Horas 
📌Mayarí 2- 1:49 Horas 
📌Velasco 1- 1:36 Horas 
📌Cto 6 de Moa(Rolo)- 1:25 Horas 
📌Banes-Banes (Ramal Esterito)- 1:12 Horas 
📌Cto Banes 2 (Ramal Tanganica)- 1:12 Horas 
📌Banes-Antilla- 0:29 Minutos
```

## Requirements

TBD — written once the open points are resolved.

## Implementation phases

TBD — defined once the requirements are finalised. Their order relative to the phases of other modules is also still to be decided.

## Open points

None so far.
