---
name: feynman
description: Validate and cement learning. After studying with /learn, verify how well you understood and fill the gaps. Optional `--lang`/`-l` chooses the language used in prompts, diagnosis, and follow-ups (default English). Examples "/feynman TCP handshake", "/feynman IAM role --lang ko", "/feynman BGP -l ja".
tags: [study, validation, recall]
requires: []
author: rainwnssystem
created: 2026-06-19
updated: 2026-06-19
---

**When to use**: to **validate · cement** a concept already learned via `/learn`.
Not for first-time learning — this is the "I thought I understood, but did I really?" test that exposes weak spots.

When the user invokes `/feynman <concept> [--lang <lang>]`, proceed through the steps **in order**.

## Invocation options

| Flag | Alias | Value | Default | Effect |
|------|-------|-------|---------|--------|
| `--lang` | `-l` | Language name or ISO code (`ko`, `Korean`, `ja`, `zh`, `English`, ...) | `English` | The Step 1 prompt to the user, the Step 2 diagnosis table and comments, the Step 3 follow-up question, the Step 4 corrections, recall questions, and the recommended-revisit line are all written in that language. |

Parse the flag from the invocation tail before treating the remainder as `<concept>`. Both `--lang ko` and `--lang=ko` (and the `-l` alias) are accepted.

- `/feynman TCP handshake --lang ko` → concept = "TCP handshake", lang = Korean
- `/feynman IAM role -l ja` → concept = "IAM role", lang = Japanese
- `/feynman BGP` → concept = "BGP", lang = English (default)

Concept names, code identifiers, command flags, protocol / RFC names stay verbatim even inside translated prose. If the language value is ambiguous or unrecognized, fall back to English and note the fallback in a one-line prefix before Step 1.

## Step 1: Make them explain first (never skip)

Since this stage **validates** already-learned content, **the AI does not explain first.**
Respond as follows (translate the quoted prompt into the language selected by `--lang`; keep `/learn` and `/feynman` as-is):

> "Please explain {concept} in your own words, as if teaching a beginner.
> Try following the `/learn` structure (scenario → prior approach → limits → core insight → mechanics → trade-offs + decision criteria).
> If you are stuck or unsure anywhere, write it honestly: 'I do not know this' / 'not confident here'."

Then wait for the user's response.

## Step 2: Diagnose

When the user writes their explanation, analyze:

- ✅ **Accurate parts** — understanding to leave as-is
- ⚠️ **Vague parts** — spots glossed over with hand-waving
- ❌ **Wrong parts** — incorrect mental model
- 🔗 **Weakest link** — the core spot whose collapse breaks overall understanding

## Step 3: Poke one weak link

Pick the most important gap and pose it as **one question**. (Never multiple at once.)
Wait for the user's answer.

## Step 4: Consolidate · cement

Based on the user's answer:
1. Present an **understanding diagnosis table** first — mark ✅/⚠️/❌ + one-line comment for each `/learn` step (scenario / prior approach / limits / insight / mechanics / trade-offs / decision criteria).
2. Correct · supplement only the weak parts (briefly acknowledge already-accurate parts, never re-explain).
3. If the user's wrong / vague parts are about **structure / flow**, reinforce with Mermaid diagrams.
   If about **conceptual differences**, reinforce with comparison tables.
4. Give 2 recall questions. This time, target the user's weak areas.
5. End with a **recommended next revisit time** (e.g., "Recommend /feynman again in 3 days").

## Principles

- Remember this is the **post-learning** stage. If the concept is new, suggest `/learn` first (translate the quoted line into the `--lang` language; carry the same flag through the suggestion):
  > "This concept seems new to you. It is more efficient to learn it first via `/learn {concept}`, then return to `/feynman`. Proceed anyway?"
- Never skip Step 1. Even if the user says "just explain it", coax them to try once.
- Diagnose with an **observational** tone, not critical. Not "you are wrong" but "you seem to understand it as ~, but actually ~".
- The goal is **validation · cementing**, not first teaching. Never re-explain what the user already got right.
