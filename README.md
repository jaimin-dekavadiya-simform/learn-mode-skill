# Learn Mode Skills

Two companion [Claude Code](https://claude.com/claude-code) skills for people who want technical
answers shaped around how they actually learn, instead of a generic explanation followed by four
rounds of "wait, but is that true?" / "explain that simply" / "what about X edge case?".

| Skill | Purpose |
|---|---|
| [`skills/learn-mode`](skills/learn-mode/SKILL.md) | Answers every substantive question with source-verified claims, a plain-language bottom line up front, proactive edge-case coverage, a real design verdict (not neutral narration), inline jargon definitions, and a closing takeaway. |
| [`skills/learn-mode-builder`](skills/learn-mode-builder/SKILL.md) | Builds a **personalized** version of `learn-mode` for a specific user by analyzing their own conversation patterns — it never copies another person's `learn-mode` verbatim; it re-derives the pattern from scratch each time. |

## Why two skills?

`learn-mode` here encodes one specific person's observed learning pattern (verify-first, bottom-line-first,
edge-case-aware, opinionated on design trade-offs). Rather than asking everyone to copy that exact
skill and hope it fits, `learn-mode-builder` reads *your own* conversation history and writes a fresh
skill tailored to *your* patterns — how you ask follow-ups, whether you want brevity or depth, whether
you want a verdict or a neutral description, and so on.

Typical flow:

1. Have a few real technical conversations with Claude.
2. Ask Claude to run `learn-mode-builder` ("build me a learn-mode skill based on this conversation").
3. It writes you a new skill (with your own name/scope choices) instead of installing this repo's
   `learn-mode` as-is.

You can also install `learn-mode` directly if its pattern already matches how you like to be
answered — see below.

## Installation

Both skills are plain [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) — a folder
containing a `SKILL.md` with YAML frontmatter (`name` + `description`) and instructions in the body.
Claude Code auto-loads any skill it finds under a skills directory; no build step or restart beyond
reloading the skill list is required.

### 1. Clone the repo

```bash
git clone git@github.com:jaimin-dekavadiya-simform/learn-mode-skill.git
cd learn-mode-skill
```

### 2. Install as user-level skills (available in every project)

Copy the skill folders into your global Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills
cp -r skills/learn-mode ~/.claude/skills/learn-mode
cp -r skills/learn-mode-builder ~/.claude/skills/learn-mode-builder
```

### 3. (Alternative) Install as project-level skills (this repo only)

From inside the project you want the skill scoped to:

```bash
mkdir -p .claude/skills
cp -r /path/to/learn-mode-skill/skills/learn-mode .claude/skills/learn-mode
cp -r /path/to/learn-mode-skill/skills/learn-mode-builder .claude/skills/learn-mode-builder
```

### 4. Verify

Start (or restart) a Claude Code session and ask a substantive technical question — `learn-mode`
should activate automatically (no need to say "explain simply" or "verify this" first). To confirm
it's picked up, you can also ask Claude directly: "which skills are available?"

To generate your own personalized variant instead of using `learn-mode` as-is, just ask:

> "Build me a learn-mode skill based on this conversation."

`learn-mode-builder` will analyze your own question patterns and write a new skill, asking you where
to save it (user-level vs. project-level) and what to name it.

## Repo layout

```
learn-mode-skill/
├── README.md
└── skills/
    ├── learn-mode/
    │   └── SKILL.md
    └── learn-mode-builder/
        └── SKILL.md
```

## License

No license specified yet — add one (e.g. MIT) if you intend for others to reuse or modify these
skills.
