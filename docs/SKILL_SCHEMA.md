# SKILL.md Schema

Every skill in this repository must follow this schema. It is the contract that CI validates and that other skill authors rely on.

## File layout

Each skill is a top-level directory at the repository root:

```
<skill-name>/
├── SKILL.md          # required — frontmatter + body
├── examples/         # optional — call examples, sample outputs
├── references/       # optional — supporting material
└── CHANGELOG.md      # optional — only when changes get noisy
```

The directory name **must** equal the `name` field in the frontmatter. When the repository is cloned into an agent's skills root (e.g. `~/.claude/skills/learn-skills/`), every such directory becomes a discoverable skill.

## Frontmatter (YAML)

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `name` | yes | string (kebab-case) | Skill identifier. Must match the directory name. |
| `description` | yes | string (single line) | What the skill does and when it triggers. Include trigger hints (usage examples). |
| `tags` | no | string[] | Search / grouping hints. |
| `requires` | no | string[] | Tools the skill expects to call (e.g. `WebSearch`, `WebFetch`, `Bash`). |
| `author` | no | string | Author handle. |
| `created` | no | ISO date (`YYYY-MM-DD`) | First-introduced date. |
| `updated` | no | ISO date (`YYYY-MM-DD`) | Last substantive change. |

### Example

```yaml
---
name: learn
description: Deep-dive into a technical concept — scenario → prior approach → limits → insight → mechanics → trade-offs → recall. Examples "/learn Raft", "/learn IAM".
tags: [study, learning, deep-dive]
requires: [WebSearch, WebFetch]
author: rainwnssystem
created: 2026-06-19
updated: 2026-06-19
---
```

## Body

After the frontmatter, write the skill's instructions in Markdown. There is no fixed body template, but:

- Lead with **when to use** (one short paragraph). The first 1–2 lines often serve as the trigger summary that downstream tooling extracts.
- Use `##` headings to delineate phases / steps.
- Prefer tables, lists, and Mermaid diagrams over walls of prose.

## Rules

- **`name` must equal the directory name.** A skill at `foo/SKILL.md` must have `name: foo`.
- **`description` is a single line.** Multi-line breaks downstream tooling that extracts trigger hints.
- **No secrets, no credentials, no private URLs.** Skills are public artifacts; treat them like documentation.
- **History lives in git, not in frontmatter.** Use `git log` and commit messages to track changes — no `version` field.

## Naming conventions

- Skill name: lowercase **kebab-case**, verb-led when possible (`learn`, `feynman`, `review-pr`).
- Avoid abbreviations that need a glossary (`rev-pr` ❌, `review-pr` ✅).
- Reserve top-level skill names — do not register one skill under another's directory.
