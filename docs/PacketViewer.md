# FFXI Addon — PacketViewer

> [!NOTE]
> **Quick Reference**
> - **Path:** `C:\Windower\addons\PacketViewer\`
> - **Author / Version:** Arcon / `1.0.0.0`
> - **Commands:** `//packetviewer` / `//pv <track|scan|log|record|display|show|eval|save|help>`
> - **Load Status:** On-Demand
> - **Origin:** Official Windower

Developer inspection tool that tracks, color-highlights, logs, records, and scans incoming and outgoing FFXI packet chunks in real time.

## What the Code Does
- Hooks `incoming chunk` and `outgoing chunk` via `register_packet(dir, id, data, modified, injected, blocked)`, routing each packet through three subsystems:
  - **Tracker (`track_packet`):** Parses packets matching `tracking[dir]` and optional `tracking.filter` field criteria using `packets.parse` and `packets.fields`. Color-codes every payload byte in a 16-column hex+ASCII dump (`string.hexformat`), checks `field.const` assertions (`CheckConst`), evaluates `field.fn` helpers, and renders the live `tracker` `texts` box.
  - **Logger (`log_packet`):** Filters by direction, mode (`known`, `unknown`, `hybrid`), and packet ID allow/exclude lists (`not ...`), outputting hex dumps (and optionally parsed fields when `LogFields` is true) to console, chatlog, or `data/logs/{full,incoming,outgoing}.log` plus per-packet `data/logs/<dir>/0xXXX.log`.
  - **Recorder & Scanner:** When `recording` is active, appends parsed packet tables to `saved_packets` for review via `display_packet(index)` and mouse wheel scrolling (`mouse` event `type == 10`). `scan_packet` searches raw payloads (offset `5+`) for packed integer (`b`/`H`/`I`) or string (`A`) values.

## Commands & Usage
- `//pv track` (`t`) `<i|o|both> <id1|id2> [field val ...]` / `stop|pause|resume|hide|once` — Live hex+field inspection (pipe-delimited IDs, optional field filters).
- `//pv log` (`l`) `<console|chatlog|file> <i|o|both> [mode] [not] <ids...>` / `stop` — Log matching packets.
- `//pv scan` (`s`) `<i|o|both> [mode] <value>` / `start|stop|mode <m>` — Scan packet payloads for numeric/string bytes.
- `//pv record` (`rec`) `[start|stop]` & `//pv display` (`u`) `<show|hide|#>` — Record tracked packets and browse them with the scroll wheel.
- `//pv show <junk|known|unknown> <true|false>` — Toggle field visibility filters and save settings.
- `//pv eval` (`e`) `<lua>` / `//pv save` — Evaluate Lua or save settings.

## Events, Packets & Technical Hooks
- **Windower Events:** `mouse`, `incoming chunk`, `outgoing chunk`, `addon command`
- **Packets:** Inspects all incoming/outgoing packet chunks (`0x000`–`0x1FF`)
- **Dependencies / Libraries:** `luau`, `texts`, `files`, `packets`, `chat`

## Local Settings & Customizations
`data/settings.xml` uses default settings (`Mode = hybrid`, `Show.{Junk,Known,Unknown} = true`, `CheckConst = true`, `LogChat = true`, `LogFields = false`, `LogTimestamp = true`, Consolas 10 display/tracker windows at `(0, 0)`).

## Related Notes
- `FFXI Windower Overview`
- `FFXI Windower Addons Catalog`
