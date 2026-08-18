# Learn Mode Builder

A skill for [Claude Code](https://claude.com/claude-code) that builds you your **own** personal
"learn-mode" skill — one that teaches Claude how you like questions answered, based on your actual
conversations. It's generated fresh for you each time, not a fixed template.

## The skill you install

| Skill | What it does |
|---|---|
| [`skills/learn-mode-builder`](skills/learn-mode-builder/SKILL.md) | Looks at your current conversation, figures out how you like to be answered, and writes a new skill file just for you. It never copies someone else's skill. |

This is the only skill you need to install. See [Installation](#installation) below.

## What's in the `examples/` folder

| File | What it is |
|---|---|
| [`examples/codebase-learn-mode`](examples/codebase-learn-mode/SKILL.md) | A sample skill the builder already generated for one person. It's here just to show you what the output looks like. |

**Don't install this example for yourself** — it was written for someone else's habits and situation.
Run `learn-mode-builder` instead and let it build a skill from your own conversation.

## How it works

1. Have a normal conversation with Claude — ask and react the way you normally would.
2. Ask Claude: *"Build me a learn-mode skill based on this conversation."*
3. The builder checks if you already have a similar skill. If this is more of the same, it improves
   that skill. If it's a genuinely different kind of learning, it creates a new, separately named skill
   instead of mixing the two.
4. It studies the conversation for your actual pattern — including whether your habits change
   depending on the kind of question. If they do, it builds one skill that switches behavior by
   question type, rather than several separate skills competing to answer the same question.
5. It asks where to save the skill and what to name it, making sure the name and description don't
   clash with anything you already have.
6. If the conversation's too short to tell your real pattern, it says so honestly instead of guessing.

## What actually makes it build a better skill

It's not conversation length that matters — it's genuine, repeated, varied reactions:

- **React the way you actually would** — push back, ask for it simpler, whatever's genuine to you.
- **Do it more than once**, so the builder can tell it's a real habit, not a one-off.
- **Ask a few different kinds of questions**, so it can tell whether your habits change by topic or
  stay the same.

A short conversation with a few clear, repeated reactions beats a long one with none.

## Installation

`learn-mode-builder` is a simple skill folder with one file, `SKILL.md`. Claude Code automatically
finds and loads any skill placed in the right folder.

### Option A — using `npx skills` (recommended)

```bash
# Install for every project you work on
npx skills add jaimin-dekavadiya-simform/learn-mode-skill -g

# Or install for just the current project
npx skills add jaimin-dekavadiya-simform/learn-mode-skill

# Check what it would install first, without installing
npx skills add jaimin-dekavadiya-simform/learn-mode-skill -l
```

### Option B — clone and copy it yourself

```bash
git clone git@github.com:jaimin-dekavadiya-simform/learn-mode-skill.git
cd learn-mode-skill

# For every project:
mkdir -p ~/.claude/skills
cp -r skills/learn-mode-builder ~/.claude/skills/learn-mode-builder

# Or just this project:
mkdir -p .claude/skills
cp -r skills/learn-mode-builder .claude/skills/learn-mode-builder
```

### Try it

Start a Claude Code session, have a real conversation, then say:

> "Build me a learn-mode skill based on this conversation."

## Repo layout

```
learn-mode-skill/
├── README.md
├── skills/
│   └── learn-mode-builder/
│       └── SKILL.md              # the skill you install
└── examples/
    └── codebase-learn-mode/
        └── SKILL.md               # example output only — don't install this
```

## License

No license yet — add one (like MIT) if you want others free to reuse or change this skill.
