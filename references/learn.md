# Learn mode

The dispatcher (`SKILL.md` at the skill root) has already parsed `<topic>` and `<lang>`. Proceed in four stages: **Collect → Synthesize → Document → Persist**.

## Output language (`<lang>`)

`<lang>` defaults to English. When non-English, the entire rendered note — section headings, prose, tables, diagram labels, callouts, recall questions, and the closing terminal confirmation — is written in that language.

What stays untranslated regardless of `<lang>`:

- **Filename** — always ASCII snake_case based on `<topic>` (see Step 4). `<lang>=ko` does not change the file name.
- **Code identifiers, command flags, API names, protocol names, RFC numbers** (`kubectl`, `--no-verify`, `O_DIRECT`, `RFC 9293`) — kept verbatim inside translated prose.
- **Reference titles and URLs** (Section 10) — kept verbatim. Only your annotations / commentary are translated.

**Core philosophy:**
- **Always enforce the "why" structure** — never enter with a definition like "X is a system that...". Start with a real-world scenario.
- **Web research is mandatory** — never write from memory alone. At least 1 WebSearch + 1 WebFetch.
- **Always include mechanics + components** — even for simple concepts, do not omit the mechanism (if absent, state "no internal mechanism — abstract model only").
- **Top-down** — not definition → structure → example, but scenario → why → how → what → when.

---

## Step 1: Multi-source Collection (Collect) — Mandatory

### Source priority

| Tier | Source | Strength |
|------|--------|----------|
| S | Official **conceptual / overview** pages (e.g., k8s "Concepts", Go blog, AWS Well-Architected pillars). NOT API reference. | Authors' intended mental model and design rationale |
| S | **Wikipedia article (read end-to-end)** | Breadth + adjacent concepts + historical context. Single best source for "concepts you did not know you should ask about". |
| A | Standards (RFCs, language specs), academic papers, top-tier conference talks | Primary-source accuracy |
| B | Authoritative engineering blogs (Brendan Gregg, Martin Kleppmann, AWS Builders) | Deep interpretation |
| C | Well-curated book excerpts; official API reference (lookup only) | Synthesis / lookup |
| D | Random blogs / Q&A answers | Verification aid only — never cite standalone |

### Required full-text reads (always WebFetch end-to-end — no snippet-only)

The lead paragraph of a Wikipedia article or an official concept page only defines the central term. Adjacent concepts — variants, successors, "see also" entries — only appear several sections in. Skimming the top loses them silently.

**Worked example.** The Wikipedia article for **TLS** introduces TLS at the top. DTLS (TLS over datagram transports) appears several sections deeper. A snippet read would teach you "TLS = secure channel" and stop. A full-text read teaches the chain TLS → DTLS → QUIC → 0-RTT, which is the actually-useful map.

Always WebFetch in full when the resource exists:

| Source | What to fetch | Why end-to-end |
|--------|---------------|----------------|
| Wikipedia article for the topic (English; fall back to another language only if the English entry is a stub) | The whole article | History · Variants · "See also" · Criticism — adjacent concepts surface here, not in the lead |
| Official conceptual / overview page (e.g., `kubernetes.io/docs/concepts/...`, `go.dev/blog/...`, AWS Well-Architected pillar). NOT API reference. | The whole page | Authors' intended mental model, design rationale, internal cross-links to neighboring concepts |

While reading end-to-end, separately capture three lists for later use:

- **Adjacent concepts surfaced** — anything that appeared without you searching for it (e.g., "TLS article mentioned DTLS, 0-RTT, ALPN"). Feeds Section 12 of the output.
- **Historical pivots** — versioning, deprecations, "previously done with X". Feeds Sections 2 and 9.
- **See also list** — kept verbatim. Feeds Section 12.

### Collection tools (use all actively)

- **WebSearch** — diversify keywords (concept name / "how X works" / "X internals" / "X vs Y" / "X architecture" / "why X over Y")
- **WebFetch** — directly fetch S/A-tier sources. Prioritize official docs.
- **MCP servers**
  - Notion: search your existing organized notes (avoid duplication + link to prior knowledge)
  - Google Drive: search reference material
- **Existing knowledge** — use it, but verify via search if the source is unclear

### Collection scope (checklist — fill all before proceeding)

- [ ] **Real-world problem scenario** — 1–2 concrete situations this technology actually solves (domain · actors · failure modes · outcome)
- [ ] **Era before this technology** — how was the same scenario solved, what was lacking
- [ ] **Origin pressure** — what market / technical pressure produced this technology
- [ ] **Core insight** — mental model compressible into 1–2 sentences
- [ ] **Internal mechanics** — mechanism, algorithm, flow
- [ ] **Key components** — components, APIs, terminology, relationships
- [ ] **Trade-offs · constraints** — what it cannot do, costs forced upon you
- [ ] **Comparison targets** — similar / alternative technologies (at least 1)
- [ ] **Decision criteria in practice** — decision axes, thresholds, anti-patterns
- [ ] **Versions · evolution** — change over time (if applicable)
- [ ] **Common misconceptions** — frequent misunderstandings
- [ ] **Adjacent concepts surfaced** — 3–5 related concepts encountered during full-text Wikipedia / official-docs reads, each tagged prerequisite / extension / variant / contrast

If collection is incomplete, **explicitly mark it** and proceed. No guessing.

---

## Step 2: Synthesis (Synthesize)

### Minimal-loss principle

- **Never drop core info** from collected material. But compress synonym repetition / decorative phrasing.
- If the same fact is expressed differently across sources → take the most accurate one + note differences in footnotes.
- For contradictions, **state both** + mark which side is more authoritative.

### Structuring

- Large concept → tree of sub-concepts.
- Temporal · causal relations → flows.
- Parallel items → tables.
- Hierarchy · relationships → diagrams.

---

## Step 3: Documentation (Document)

The structure below is shown in English as the canonical reference. When `<lang>` is non-English, translate every heading, label, and prose block into that language while keeping the same section order, the same callouts, and the same mandatory sub-sections (e.g., Section 7-1 Decision criteria, Section 12 Adjacent map mini-blocks).

### Output structure (required sections — fixed order)

```
# {Topic}

## 0. Overview
3–5 lines. What · why · one core point.

## 1. What real-world problem does it solve (required · right after Overview)
- 1–2 concrete scenarios. Format: "Suppose you're building ~. Here is what happens..."
- Specify domain · actors · numbers · failure modes. No standalone abstract nouns ("at scale", "highly available").
- This scenario is the reference point for all later sections — when explaining mechanics · comparisons · constraints, return to it and instantiate.

## 2. Before (prior approach)
How was this scenario solved before this technology existed. Concrete examples · cases.

## 3. Pressure (limits and pressure)
How the prior approach broke down in that scenario. What was broken or painful.
Connect to changes in market · scale · operating environment.

## 4. Insight (core insight)
What conceptual leap elegantly resolves that scenario.
Compress into a 1–2 sentence mental model.

## 5. How (mechanics)
- Step-by-step flow (Mermaid sequence / flowchart recommended)
- 1–2 line description per step
- Pull scenario #1 back in and instantiate "how this step handles that situation"
- If the internal mechanism is abstract (concept / protocol / etc.), state "no physical mechanism — convention / contract"

## 6. Components
- Component table (name / role / note)
- Relationship diagram (Mermaid — required if 2+ components)
- If no components (pure abstract concept), state "no components — single algorithm / rule"

## 7. Trade-offs
- Comparison table: prior vs new approach (markdown table required)
- What this technology is bad at, forced costs, situations where it should not be used
- State 1 "scenario this technology also cannot solve" → concretize the limit

### 7-1. Decision criteria in practice — required sub-section
A comparison table alone is insufficient. Write a separate block on **"how do you decide in practice"**:

- **3–5 decision axes** — what do you look at to judge
  (e.g., write / read ratio, data size, consistency requirements, team ops capability, cost model, lock-in, latency SLA, regulations)
- **Concrete thresholds** — in numbers when possible
  ("under 10k QPS use A, above consider B", "if write ratio exceeds 30% X is unsuitable")
- **Decision table** — situation → recommendation → reason
  ```
  | Situation | Recommended | Reason |
  |-----------|-------------|--------|
  | Startup MVP, unpredictable traffic | A | Lower ops burden |
  | 50k QPS, global multi-region | B | A's single-region limit |
  ```
- **Anti-patterns** — "if you pick X in this case you will regret it", 1–2 entries. Call out common wrong-choice reasoning.
- **Industry pattern (optional)** — if there is a noticeable trend in real company choices, 1–2 lines.

## 8. Common misconceptions
- Wrong belief → actual

## 9. Evolution (if applicable)
- Version changes · timeline

## 10. References
- S / A-tier source links (mandatory — at least 2)

## 11. Recall questions
- 3 questions. Focused on "why / when / how", not "what".
- At least 1 must be a **variation of scenario #1** to test application.
- At least 1 must be a **scenario-based question on the 7-1 decision criteria**.
- No answers provided. Grade · supplement when the user answers.

## 12. Adjacent map (from full-text reads)
- 3–5 concepts surfaced from the Wikipedia article and the official conceptual page.
- Tag each: `(prerequisite)` — must know first / `(extension)` — next depth in same domain / `(variant)` — same problem, different trade-off / `(contrast)` — often confused with the topic.
- Each entry is a mini-block with three required sub-bullets — **How it connects**, **Why it exists**, **Next**. Example:

  - DTLS (variant)
    - **How it connects**: TLS depends on TCP's ordered, reliable byte stream. DTLS strips that assumption and redesigns the handshake and record layer to tolerate packet loss and reorder.
    - **Why it exists**: real-time traffic (VoIP, WebRTC, gaming) needs encryption but cannot pay TCP's head-of-line blocking cost. TLS-over-TCP left this market uncovered.
    - **Next**: `/learn DTLS` — how reliability illusions are rebuilt over UDP without retransmitting the whole stream.

- These are the candidates for the next `/learn`, `/learn ... --feynman`, or `/read-paper` session.
```

### Readability tools (use actively)

Instead of continuous prose, combine **varied elements**:

| Tool | When |
|------|------|
| **Comparison table** | 2+ comparisons, change-points, trade-offs, listing options — **2+ items → always a table** |
| **Mermaid flowchart** | Decision flow, component relationships |
| **Mermaid sequence** | Time-ordered interactions (request-response, consensus protocol) |
| **Mermaid state** | State transitions |
| **Bullet list** | 3+ parallel items |
| **Callout (`> **NOTE**:`)** | Important pitfalls, key emphasis |
| **inline code** (`like_this`) | API names, flags, commands |
| **Bold** | 1–2 per paragraph max. Real key only |
| **Code block** | Concrete examples (config, command, snippet) |
| **Numbered list** | Only when order matters |

### Writing rules

- **Paragraphs ≤ 3 lines**. Beyond that, split or switch to a tool.
- **No adjacent tool repetition** — no table after table. Rhythm: table → diagram → short prose.
- **No decoration** — no emoji / fluff. Exception: ✅⚠️❌ for semantic conveyance.
- **No abstract-noun stacking** — empty sentences like "provides scalability, availability, reliability". Use concrete examples.

---

## Step 4: Persist (Persist) — Mandatory

The rendered note must end up in a file the user can return to. Terminal output alone is treated as **incomplete**.

### Where to write

| Decision | Rule |
|----------|------|
| Directory | The **current working directory** the user ran `/learn` in. Do not create subdirectories, do not write elsewhere. |
| Filename | `<topic>.md`, where `<topic>` is the user's input converted to **snake_case**: lowercase, ASCII, whitespace and `-` collapsed to `_`, punctuation dropped. `AWS IAM` → `aws_iam.md`, `Raft consensus` → `raft_consensus.md`, `TCP/IP` → `tcp_ip.md`. |
| Collision | If `<topic>.md` already exists, append `_2`, `_3` (e.g., `tls_2.md`) rather than overwriting. Never silently overwrite an existing learn note. |

### What to write

- **Exactly** the rendered Section 0–12 document, in the language selected by `<lang>` (default English). No truncation, no "see terminal for full version".
- File body starts with the `# {Topic}` heading; no extra preamble. The `{Topic}` itself stays as the user typed it (do not translate proper-noun topics like "Raft consensus" or "AWS IAM").
- Use the `Write` tool. Confirm the final path back to the user as the closing line of the terminal response, in the same language as the note body:

  > Saved to `tls.md`.  *(English)*
  > `tls.md`에 저장했습니다.  *(Korean, `<lang>=ko`)*

### Failure modes to avoid

- Writing only a summary to the file and keeping the full body in the terminal — defeats the purpose.
- Writing to a path outside the current working directory, or under a `notes/`-style subfolder, without being asked.
- Skipping persistence when the topic is "small" — every `/learn` invocation produces a file.

---

## Quality checklist (run before output — fix and re-output on any miss)

- [ ] **Is there a "what real-world problem" section right after Overview, with concrete scenarios (domain · actors · numbers)?**
- [ ] **Is that scenario referenced again in Mechanics · Comparison · Constraints sections?** (Not used once and discarded.)
- [ ] **Does the "why" narrative flow unbroken: prior approach → limits → core insight?**
- [ ] **Did you actually execute WebSearch + WebFetch and cite at least 2 S / A-tier sources?**
- [ ] **Did you WebFetch the Wikipedia article and the official conceptual page end-to-end (not just snippet-search)?**
- [ ] **Does the Adjacent map (Section 12) list 3–5 surfaced concepts, each as a mini-block with `How it connects` · `Why it exists` · `Next`?**
- [ ] **Does the Mechanics section exist with an actual mechanism, not just definitions?**
- [ ] **Does the Components section exist with a component table + (if applicable) relationship diagram?**
- [ ] At least 1 comparison target specified?
- [ ] At least 1 Mermaid diagram?
- [ ] Trade-offs section in comparison-table form?
- [ ] **Does the Decision Criteria sub-section exist with decision axes + thresholds + decision table + anti-patterns?**
- [ ] Any paragraph exceeds 3 lines?
- [ ] Is the "Common misconceptions" section non-empty?
- [ ] Of the 3 recall questions: 1 is a scenario variation, 1 is a decision-criteria scenario-based question?
- [ ] **Was the full rendered note written to `<topic>.md` in the current working directory via `Write`, and was the path echoed back to the user?**
- [ ] **Is the entire body in the language requested by `<lang>` (default English), with code identifiers / API names / reference titles kept verbatim?**

---

## Prohibited

- Writing from memory without searching. **WebSearch + WebFetch each must execute at least once.**
- Assertions without sources.
- Writing speculation as fact. If uncertain, mark "needs verification".
- Filling an entire page with prose only.
- **Entering with abstract definitions like "X is a system that ~"**. Right after Overview must be a real-world scenario.
- **Explaining mechanics without a scenario** — the answer to "why should I know this?" must come before the reader asks.
- **Skipping the "why" sections (2–4) and jumping to "how" (5–6)** — showing only mechanism leads to evaporation in days.
- **Showing usage · commands · code first** — code comes after concept explanation.
- **Snippet-only reading of Wikipedia or the official conceptual page.** Adjacent concepts that only appear mid-article will be silently lost. Full-text WebFetch is required.
- **Terminal-only output.** Every `/learn` run must persist the rendered note to `<topic>.md` in the current working directory. Skipping `Write` leaves the user with a scroll buffer instead of a reusable note.
