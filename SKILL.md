---
name: learn
description: Default mode deep-dives a technical concept, persists `<topic>.md` (scenario → why → how → trade-offs → recall), then stays open for follow-up Q&A answered from the gathered sources — no re-attach needed. `--feynman` switches to post-learning validation (explain-back + gap diagnosis). Optional `--lang`/`-l` chooses output language (default English). Examples "/learn Raft consensus", "/learn AWS IAM --lang ko", "/learn TCP handshake --feynman", "/learn IAM role --feynman -l ja".
tags: [study, learning, deep-dive, validation, recall]
requires: [WebSearch, WebFetch, Write, Read]
author: rainwnssystem
created: 2026-06-19
updated: 2026-06-19
---

When the user invokes `/learn`, do **not** act from memory. The full instructions live in `references/`; load only the one file that matches the requested mode so token usage stays minimal.

## Step 1: Parse the invocation

Strip flags from the tail of the user's input. Accept both `--flag value` and `--flag=value` forms.

| Flag | Alias | Value | Default |
|------|-------|-------|---------|
| `--lang` | `-l` | Language name or ISO code (`ko`, `Korean`, `ja`, `zh`, `English`, ...) | `English` |
| `--feynman` | — | (boolean — no value) | unset |

Everything remaining after the flags is `<topic>` (or `<concept>` in Feynman mode). If `--lang` is ambiguous or unrecognized, fall back to English and note the fallback in a one-line prefix in the terminal response (not in any saved file).

### Examples

| Invocation | Mode | Subject | Lang |
|------------|------|---------|------|
| `/learn Raft consensus` | learn | "Raft consensus" | English |
| `/learn AWS IAM --lang ko` | learn | "AWS IAM" | Korean |
| `/learn Linux cgroup -l ja` | learn | "Linux cgroup" | Japanese |
| `/learn TLS --lang=zh` | learn | "TLS" | Chinese |
| `/learn TCP handshake --feynman` | feynman | "TCP handshake" | English |
| `/learn IAM role --feynman -l ja` | feynman | "IAM role" | Japanese |

## Step 2: Dispatch — Read exactly one reference file

- **If `--feynman` is present** → use `Read` to load `references/feynman.md` (sibling of this `SKILL.md`), then follow it end-to-end with the parsed `<concept>` and `<lang>`.
- **Otherwise** → use `Read` to load `references/learn.md`, then follow it end-to-end with the parsed `<topic>` and `<lang>`.

Do not load both. Do not attempt either flow from memory — the reference file is the authoritative instruction set for that mode.
