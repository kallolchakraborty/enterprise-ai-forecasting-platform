---
name: headroom
description: >
  Token-reduction skill for coding agents. Compresses JSON tool output, build
  logs, grep/ripgrep search results, source code, and conversation history the
  same way the Headroom proxy does — in-context, no binary install. Also shapes
  the agent's own output (skip ceremony, don't restate code already on screen,
  drop reasoning on routine turns).
license: Apache-2.0
compatibility: opencode
metadata:
  source: https://github.com/roman-ryzenadvanced/headroom-skill
---

# Headroom — portable token-reduction skill

## When to apply

| Content the agent is about to use / paste | Apply | Helper script |
|---|---|---|
| JSON array of dicts (tool results, API) | SmartCrusher | `scripts/smart_crusher.py` |
| Build / test logs (pytest, npm, cargo) | LogCompressor | `scripts/log_compressor.py` |
| grep / ripgrep / ag output | SearchCompressor | `scripts/search_compressor.py` |
| Source code (Python, JS, TS, Go, Rust, Java, C/C++) | CodeCompressor | `scripts/code_compressor.py` |
| Conversation over token budget | RollingWindow | `scripts/context_manager.py` |
| Output the model writes back | Output Shaper (L2) | `prompts/verbosity_steering.md` |

## Seven rules

1. **Compress before paste** — over ~200 tokens, compress with matching transform. Narrate what you did.
2. **Never drop errors** — preserve errors, stack traces, `is_error: true` results 100%.
3. **Keep first N and last N** — always keep first 3, last 2 items.
4. **Stabilise the prefix** — keep system prompt byte-identical. Move dynamic content to tail.
5. **Drop oldest tool outputs first** — drop oldest `tool_call` + `tool_result` pairs. Never split pairs.
6. **Shape your own output** — L2 default: skip preambles, reference by `path:line`, don't restate code.
7. **Be reversible** — write originals to `.headroom-cache/<sha>.txt`, cite the SHA.

## Output verbosity levels

| Level | Behavior |
|-------|----------|
| L0 | Off. Write naturally. |
| L1 | Skip preambles ("Great, let me…") and postambles. |
| L2 | L1 + don't restate code already on screen. Reference by `path:line`. **(default)** |
| L3 | L2 + conclusions only. Skip reasoning unless asked. |
| L4 | Fragments. Minimum tokens. No full sentences unless safety requires. |

## Effort routing

| Last turn | Verbosity | Effort |
|-----------|-----------|--------|
| New user ask | L2 | full |
| `tool_result`, all `is_error: false` | L3 | low |
| `tool_result` with `is_error: true` | L2 | full |

## Safety rules

1. Never drop user or assistant text.
2. Never split a `tool_call` / `tool_result` pair.
3. Parse failures are no-ops — pass through unchanged.
4. Always preserve recency (last 2 user turns).
5. Always preserve the system prompt.
6. 100% error preservation.
7. Idempotent — compressing already-compressed output is a no-op.

## Selection heuristics (priority order)

1. Errors
2. First 3 items
3. Last 2 items
4. Anomalies (>2σ from mean, or differs from modal value)
5. Relevant (top-K by BM25 to user's question)
6. Change points (status flips, counter resets)
7. Sample (10% of remainder, max 20 items)
