# Learn Skills

A single Markdown-based agent skill focused on **deep diving and retention** of computer-science fundamentals, cloud computing, and backend engineering.

The skill exposes two modes from one entry point:

| Mode | Trigger | What it does |
|------|---------|--------------|
| Learn (default) | `/learn <topic>` | Deep-dive into a technical concept with a strict "scenario → why → how → trade-offs → recall" structure. Web research is mandatory. Persists `<topic>.md` to the current working directory, then stays open for follow-up Q&A answered from the sources already gathered — no re-attach required. |
| Feynman | `/learn <concept> --feynman` | Post-learning validation. Forces you to explain in your own words, then diagnoses gaps. |

Both modes accept `--lang`/`-l` for output language (default English).

After Learn mode saves the note, the same session stays open for follow-up questions. Read along, ask anything, and the agent answers from the corpus it just collected (citing the note's section anchors). It only re-searches when a question genuinely falls outside the gathered material, and offers to patch the saved note via `Edit` whenever a correction surfaces.

The repository follows the schema in [`docs/SKILL_SCHEMA.md`](./docs/SKILL_SCHEMA.md). At install time the repo becomes a single skill directory; the dispatcher `SKILL.md` loads exactly one of `references/learn.md` or `references/feynman.md` per invocation, so token usage stays minimal regardless of mode.

## Install

Clone the repository **directly as the skill directory** in your agent's skills root. The second argument to `git clone` renames the destination so `SKILL.md` lands exactly one level under the skills root.

### Claude Code

```bash
git clone https://github.com/rainwnssystem/learn-skills.git ~/.claude/skills/learn
```

### Codex

```bash
git clone https://github.com/rainwnssystem/learn-skills.git ~/.codex/skills/learn
```

### OpenCode

```bash
git clone https://github.com/rainwnssystem/learn-skills.git ~/.config/opencode/skills/learn
```

### Custom skills directory

```bash
git clone https://github.com/rainwnssystem/learn-skills.git <your-skills-dir>/learn
```

### Update

```bash
cd ~/.claude/skills/learn && git pull
```

### Use

Invoke as a slash command:

```
/learn Raft consensus
/learn AWS IAM --lang ko
/learn TCP handshake --feynman
/learn IAM role --feynman -l ja
```

## Repository layout

```
.
├── README.md
├── LICENSE
├── SKILL.md                     # dispatcher: flag parsing → loads one reference
├── references/
│   ├── learn.md                 # learn-mode body (Collect → Synthesize → Document → Persist)
│   └── feynman.md               # feynman-mode body (Explain → Diagnose → Poke → Consolidate)
└── docs/
    ├── SKILL_SCHEMA.md          # frontmatter and authoring rules
    └── skill-template/SKILL.md  # copy this to start a new skill
```

After cloning into `~/.claude/skills/learn`, the agent discovers `~/.claude/skills/learn/SKILL.md` directly — no extra nesting.

## Authoring notes

- The dispatcher is intentionally thin. Mode bodies live in `references/` so an invocation only pays for the file it actually needs.
- Frontmatter and body language: English (the published skill surface is English-first). User-facing prose is translated at run time via `--lang`.
- Skill history lives in `git log`. No `version` field, no version mentions in commit messages.

## License

MIT — see [LICENSE](./LICENSE).
