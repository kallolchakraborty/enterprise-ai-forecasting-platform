# Headroom token-reduction rules

> Compress tool outputs before pasting. Shape your own output to L2.

## Rule 1 — Compress before paste

Before pasting any tool result over ~200 tokens, compress it:

| Content | Tool |
|---------|------|
| JSON arrays | `python scripts/smart_crusher.py` |
| Build/test logs | `python scripts/log_compressor.py` |
| grep/ripgrep output | `python scripts/search_compressor.py` |
| Source code | `python scripts/code_compressor.py` |
| Oversized context | `python scripts/context_manager.py` |

Always write originals to `.headroom-cache/<sha>.txt` first.

## Rule 2 — Never drop errors

Errors, stack traces, `FAILED`, `ERROR` lines kept 100%.

## Rule 3 — Keep first 3, last 2

For arrays, logs, search results.

## Rule 4 — Stabilise prefix

System prompt byte-identical. Dynamic content at tail.

## Rule 5 — Drop oldest tool outputs

Drop oldest `tool_call`+`tool_result` pairs first. Never split pairs.

## Rule 6 — L2 output

Skip preambles/postambles. Don't restate code on screen. Reference by `path:line`. Show smallest edit.

## Rule 7 — Reversible

Write originals to `.headroom-cache/`. Cite SHA.
