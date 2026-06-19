# Learn Skills

A curated set of Markdown-based agent skills focused on **deep diving and retention** of computer-science fundamentals, cloud computing, and backend engineering.

Each skill is a `SKILL.md` file (YAML frontmatter + body) following the schema in [`docs/SKILL_SCHEMA.md`](./docs/SKILL_SCHEMA.md). Any agent harness that loads skills from a directory of `SKILL.md` files will pick these up.

## Available skills

| Skill | What it does |
|-------|--------------|
| [`learn`](./learn/SKILL.md) | Deep-dive into a technical concept with a strict "scenario → why → how → trade-offs → recall" structure. Web research is mandatory. |
| [`feynman`](./feynman/SKILL.md) | Post-learning validation. Forces you to explain in your own words, then diagnoses gaps. |

## Install

Clone the repository directly into your agent's skills directory. Each skill (`learn/`, `feynman/`) sits at the repo root, so the cloned tree is immediately discovered. Pick the section that matches your agent.

### Claude Code

```bash
git clone https://github.com/rainwnssystem/learn-skills.git ~/.claude/skills/learn-skills
```

### Codex

```bash
git clone https://github.com/rainwnssystem/learn-skills.git ~/.codex/skills/learn-skills
```

### OpenCode

```bash
git clone https://github.com/rainwnssystem/learn-skills.git ~/.config/opencode/skills/learn-skills
```

### Custom skills directory

```bash
git clone https://github.com/rainwnssystem/learn-skills.git <your-skills-dir>/learn-skills
```

### Update

```bash
cd <wherever you cloned it>
git pull
```

### Use

Invoke as slash commands:

```
/learn Raft consensus
/feynman TCP handshake
```

## Repository layout

```
.
├── README.md
├── LICENSE
├── learn/
│   └── SKILL.md
├── feynman/
│   └── SKILL.md
└── docs/
    ├── SKILL_SCHEMA.md           # Required frontmatter and rules
    └── skill-template/SKILL.md   # Copy this to start a new skill
```

Each top-level directory that contains a `SKILL.md` is one installable skill. `docs/` is meta-documentation only.

## Contributing a new skill

1. Copy `docs/skill-template/` to `<your-skill>/` at the repo root.
2. Fill in the frontmatter following [`SKILL_SCHEMA.md`](./docs/SKILL_SCHEMA.md).
3. Write the body. Use tables, lists, and Mermaid diagrams — avoid walls of prose.
4. Open a pull request.

## License

MIT — see [LICENSE](./LICENSE).
