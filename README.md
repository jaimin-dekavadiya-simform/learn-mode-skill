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
| [`examples/codebase-learn-mode`](examples/codebase-learn-mode/SKILL.md) | A **sample output** of `learn-mode-builder` — one specific person's personalized skill for deep-diving a real codebase, kept here purely as a worked example of the shape and level of detail the builder produces (including the within-conversation category routing described below). |

**This is not a skill you should install for yourself.** It encodes one particular person's observed
pattern for *codebase-understanding conversations specifically* (source-verify hard on mechanism
questions, skip the ceremony on plain definitions, give an opinionated verdict on design-critique
questions, persist substantial findings as docs) — it will very likely be wrong for how *you* ask
questions, and it's scoped to a context (a specific codebase, source-anchored verification) that may
not even apply to what you're doing. If you copy it as-is, you're installing someone else's habits for
someone else's context, not your own. The whole point of this repo is that you run
`learn-mode-builder` and get a skill derived from *your* conversation instead.

Note this example is itself scoped to one context — "understanding a real codebase by reading its
source." The builder produces a **separate, distinctly-named skill** for a different context (e.g.
general conceptual/technical learning not anchored to a specific repo) rather than folding everything
into one skill — see "How it works" below.

## How it works

1. Have a few real technical conversations with Claude — ask questions the way you normally would,
   and react the way you normally would to the first answer (push back, ask for simpler terms, ask
   about edge cases, whatever's genuine to you).
2. Ask Claude to run the builder: *"Build me a learn-mode skill based on this conversation."*
3. The builder checks whether you already have a learn-mode-style skill installed and whether this
   conversation is more signal for that one, or a **genuinely separate context** (e.g. codebase
   understanding vs. learning an unrelated technical skill — different verification substrate,
   different natural scope) — only the latter produces a new, distinctly-named skill rather than
   folding into an existing one.
4. It then reads back over the conversation, identifies your actual recurring pattern — including
   whether that pattern itself varies by *category of question within this one conversation* (e.g.
   verify-hard on mechanism questions, quick and plain on definitions, opinionated on design critique) —
   and writes a new skill file: one file, with an internal "calibrate by category" section if the
   evidence supports it, never split into multiple competing files for within-conversation categories.
5. It asks you where to save it (user-level vs. project-level) and what to name it — and if it's
   coexisting with another learn-mode-style skill, names and describes both so their territory doesn't
   overlap.
6. If the conversation so far is too thin to confidently detect a real pattern, the builder says so
   plainly instead of inventing one, and either asks you directly what to emphasize or offers to build
   a lighter starting skill to refine later.

`examples/codebase-learn-mode` is what step 4-5 actually produced for one real codebase-understanding
conversation — read it to see the category-routing section in practice.

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
`examples/codebase-learn-mode` for you to use directly.

## Repo layout

```
learn-mode-skill/
├── README.md
├── skills/
│   └── learn-mode-builder/
│       └── SKILL.md              # the skill you install
└── examples/
    └── codebase-learn-mode/
        └── SKILL.md               # sample output only — not for installation
```

## License

No license specified yet — add one (e.g. MIT) if you intend for others to reuse or modify this skill.
