---
cairn: log
change: message-arg-stdin-utf8
landed: 2026-10-12
---

# Piped raw messages are refused when not valid UTF-8

Reported in #786: a raw message piped to `message send` with an invalid UTF-8 byte in its body was sent cut off at the line before that byte, and the command reported success. `MessageArg` read piped stdin line by line through `map_while(Result::ok)`, which stops silently at the first line that fails to decode. An invalid byte on the first line left nothing, reported as `Message is empty`.

Piped stdin is now read whole, as a message file already was, and a read error or invalid UTF-8 fails the command. The input keeps its trailing line ending, which the line join used to drop.

Spec updated: `commands` (MODIFIED: "Raw message input is shared").
