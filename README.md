# Learn Mode Builder

A [Claude Code](https://claude.com/claude-code) skill that builds you a **personalized**
"learn-mode"-style answering skill — one shaped around how *you* actually ask questions and expect
answers, derived from your own live conversation, not templated from anyone else's.

## The one skill you actually install

| Skill | Purpose |
|---|---|
| [`skills/learn-mode-builder`](skills/learn-mode-builder/SKILL.md) | Analyzes the invoking user's own question-asking and understanding patterns in the **current conversation**, then writes a fresh, personalized answering skill tailored to them. Never copies another person's `learn-mode` verbatim — every run re-derives the pattern from scratch. |

This is the only skill meant to be installed for general use. See [Installation](#installation) below.

## What's in `examples/`

| File | Purpose |
|---|---|
| [`examples/learn-mode`](examples/learn-mode/SKILL.md) | A **sample output** of `learn-mode-builder` — one specific person's personalized skill, kept here purely as a worked example of the shape and level of detail the builder produces. |

**This is not a skill you should install for yourself.** It encodes one particular person's observed
learning pattern (verify-first, bottom-line-first, edge-case-aware, opinionated on trade-offs) — it
will very likely be wrong for how *you* ask questions and want to be answered. If you copy it as-is,
you're installing someone else's habits, not your own. The whole point of this repo is that you run
`learn-mode-builder` and get a skill derived from *your* conversation instead.

## How it works

1. Have a few real technical conversations with Claude — ask questions the way you normally would,
   and react the way you normally would to the first answer (push back, ask for simpler terms, ask
   about edge cases, whatever's genuine to you).
2. Ask Claude to run the builder: *"Build me a learn-mode skill based on this conversation."*
3. The builder reads back over the conversation, identifies your actual recurring pattern (not a
   guess, not a template), and writes a new skill file — asking you where to save it (user-level vs.
   project-level) and what to name it.
4. If the conversation so far is too thin to confidently detect a real pattern, the builder says so
   plainly instead of inventing one, and either asks you directly what to emphasize or offers to build
   a lighter starting skill to refine later.

## Installation

`learn-mode-builder` is a plain [Agent Skill](https://docs.claude.com/en/docs/claude-code/skills) — a
folder containing a `SKILL.md` with YAML frontmatter (`name` + `description`) and instructions in the
body. Claude Code auto-loads any skill it finds under a skills directory; no build step is required.

### 1. Clone the repo

```bash
git clone git@github.com:jaimin-dekavadiya-simform/learn-mode-skill.git
cd learn-mode-skill
```

### 2. Install as a user-level skill (available in every project)

```bash
mkdir -p ~/.claude/skills
cp -r skills/learn-mode-builder ~/.claude/skills/learn-mode-builder
```

### 3. (Alternative) Install as a project-level skill (this repo only)

```bash
mkdir -p .claude/skills
cp -r /path/to/learn-mode-skill/skills/learn-mode-builder .claude/skills/learn-mode-builder
```

### 4. Use it

Start (or restart) a Claude Code session, have a real conversation, then ask:

> "Build me a learn-mode skill based on this conversation."

The builder will produce a **new** skill file specific to you — it will not install or suggest
`examples/learn-mode` for you to use directly.

## Repo layout

```
learn-mode-skill/
├── README.md
├── skills/
│   └── learn-mode-builder/
│       └── SKILL.md          # the skill you install
└── examples/
    └── learn-mode/
        └── SKILL.md           # sample output only — not for installation
```

## License

No license specified yet — add one (e.g. MIT) if you intend for others to reuse or modify this skill.
