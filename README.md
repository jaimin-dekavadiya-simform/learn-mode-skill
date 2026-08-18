# Learn Mode Builder

This is a skill for [Claude Code](https://claude.com/claude-code). It builds you your **own**
personal "learn-mode" skill — one that teaches Claude how *you* like questions answered, based on how
you actually talk in your own conversations. It's not a fixed template; it's generated fresh for you
each time.

## The skill you install

| Skill | What it does |
|---|---|
| [`skills/learn-mode-builder`](skills/learn-mode-builder/SKILL.md) | Looks back at your current conversation, figures out how you like to be answered, and writes a new skill file just for you. It never copies someone else's skill — it works out your pattern from scratch every time. |

This is the only skill you need to install. See [Installation](#installation) below.

## What's in the `examples/` folder

| File | What it is |
|---|---|
| [`examples/codebase-learn-mode`](examples/codebase-learn-mode/SKILL.md) | A sample skill that `learn-mode-builder` already generated for one person, for one specific use case (understanding a codebase). It's here just so you can see what the builder produces. |

**Don't install this example for yourself.** It was written for one specific person and one specific
situation (reading through a real codebase). Your own habits are almost certainly different. Instead
of copying it, just run `learn-mode-builder` and let it build a skill from *your* conversation.

Also note: this example only covers one kind of situation — "understanding a codebase." If you use
the builder for a completely different kind of learning (say, picking up a new general skill that
isn't tied to any codebase), it will create a **second, separately named** skill for that instead of
mixing the two together. More on this below.

## How it works

1. Have a normal conversation with Claude — ask questions the way you normally do, and react to the
   answers the way you normally would.
2. Ask Claude: *"Build me a learn-mode skill based on this conversation."*
3. First, the builder checks if you already have a similar skill installed. If this conversation is
   just more of the same kind of thing, it improves your existing skill. If it's a genuinely different
   kind of learning (like a different topic that needs different handling), it creates a **new, separately
   named** skill instead of mixing the two together.
4. Then it studies the conversation and figures out your actual pattern — for example, do you want
   deep verification for technical questions but a quick plain answer for simple definitions? If your
   habits differ depending on the kind of question, it builds that in as one skill with built-in
   "switch modes depending on the question" logic — not several separate skills fighting to answer the
   same question.
5. It asks you where to save the skill (just for one project, or for everything you do) and what to
   name it. If you already have another similar skill, it makes sure the new one's name and description
   don't overlap or clash with it.
6. If your conversation so far is too short to tell your real pattern, it says so honestly instead of
   guessing — and either asks you a couple of quick questions, or builds a simple starting version you
   can refine later.

Want to see this in action? `examples/codebase-learn-mode` is a real skill the builder produced this
way — open it to see what a finished, personalized skill looks like.

## What actually makes it build a better skill

It's **not** just having a longer conversation. A long conversation with no real reactions in it (you
just say "ok, thanks" every time) gives the builder almost nothing to work with. What actually helps:

- **React the way you genuinely would**, not just ask questions — push back, ask for it simpler, ask
  "but what about X," whatever you'd normally do. That reaction is the actual signal.
- **Do it more than once.** One pushback could be a one-off; the same reaction showing up two or three
  times is what tells the builder it's a real habit, not a fluke.
- **Ask a few different kinds of questions**, not just one kind. That's the only way the builder can
  tell whether your habits change depending on the topic (e.g. you verify hard on code but want quick
  plain answers on definitions) versus staying the same no matter what.

So: a shorter conversation with a few clear, repeated reactions beats a much longer one where you never
react the same way twice.

## Installation

`learn-mode-builder` is a simple skill folder containing one file, `SKILL.md`. Claude Code
automatically finds and loads any skill placed in the right folder — nothing to build or configure.

### Option A — using `npx skills` (recommended, easiest)

[`npx skills`](https://www.npmjs.com/package/skills) can install the skill straight from GitHub — no
need to clone the repo yourself.

```bash
# Install for every project you work on
npx skills add jaimin-dekavadiya-simform/learn-mode-skill -g

# Or install for just the current project
npx skills add jaimin-dekavadiya-simform/learn-mode-skill

# Just want to check what it would install first? Use -l
npx skills add jaimin-dekavadiya-simform/learn-mode-skill -l
```

It only picks up `learn-mode-builder` — the `examples/` folder is correctly ignored.

### Option B — clone and copy it yourself

**1. Clone the repo:**

```bash
git clone git@github.com:jaimin-dekavadiya-simform/learn-mode-skill.git
cd learn-mode-skill
```

**2. Copy it so it works in every project:**

```bash
mkdir -p ~/.claude/skills
cp -r skills/learn-mode-builder ~/.claude/skills/learn-mode-builder
```

**2 (alternative). Or copy it so it only works in one project:**

```bash
mkdir -p .claude/skills
cp -r /path/to/learn-mode-skill/skills/learn-mode-builder .claude/skills/learn-mode-builder
```

### Try it

Start a Claude Code session, have a real conversation, then say:

> "Build me a learn-mode skill based on this conversation."

It will write a **new** skill made for you — it won't just hand you `examples/codebase-learn-mode` to
use as-is.

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

No license yet — add one (like MIT) if you want other people to be free to reuse or change this skill.
