---
name: learn-mode-builder
description: Use when someone asks to build, generate, or create a personalized "learn-mode"-style answering skill for THEMSELVES — e.g. "build me a learn-mode skill", "generate a personalized answering skill based on this conversation", "make a skill like that other user's learn-mode but for me". Analyzes the CURRENT conversation for the invoking user's own question-asking and understanding patterns, then writes a fresh, personalized skill tailored to them. Never copies another user's learn-mode verbatim — every run re-derives the pattern from scratch.
---

# Learn-Mode Builder

This skill produces a **new, personalized** `learn-mode`-style skill for whoever is running it right
now, built entirely from patterns observed in *their own* current conversation. It does not template
or copy any specific prior learn-mode skill's content — the source material each time is the live
transcript, not a fixed example.

## Step 1 — Read the actual conversation, don't assume

Look back over this conversation's turns and identify the invoking user's own recurring behavior:

- After a first answer, what do they usually ask next? (e.g., "is that verified?", "explain simply",
  "what about X edge case?", "is that good design?", "what does that term mean?", "am I right that...?")
- Do they prefer brevity or depth? Do they want the short answer first, or do they want to be walked
  through reasoning?
- Do they push back on claims, ask for source verification, or generally take answers at face value?
- Do they like concrete examples/worked scenarios, or are abstract explanations enough for them?
- Do they want an evaluative verdict ("is this good/bad") or purely descriptive answers?
- Do they ask for things to be saved/documented, or is this purely conversational?
- Any domain-specific quirks — e.g. they always want file:line citations, they always want a diagram,
  they always want a one-line takeaway at the end?

**Honesty safeguard — do not fabricate a pattern from a thin conversation.** If the conversation so far
is too short (a handful of exchanges, or all on unrelated one-off topics) to confidently identify a real
recurring pattern, say so plainly instead of inventing one. In that case, either:
- ask the user directly what they'd want emphasized (a couple of concrete questions is fine — this is
  exactly the kind of case-specific, otherwise-unknowable decision worth asking about), or
- offer to build a lighter, more general-purpose starting skill now and refine it after a few more
  real exchanges.

Never assume the user's pattern resembles any other specific person's learn-mode skill. Every user is
different — some want terse answers, some want maximal rigor, some want reassurance rather than
critique. Derive the pattern from evidence in front of you.

## Step 2 — Synthesize the pattern into the same shape as a learn-mode skill

Once you have real signal, write:

1. **The recurring counter-questions this replaces** — 4-6 concrete follow-ups this specific user tends
   to ask, phrased in their own words where possible (quote or closely paraphrase actual things they
   said in this conversation).
2. **How to answer instead** — a numbered list of concrete behaviors that front-load those follow-ups
   into the first response. Ground each instruction in what was actually observed, not a generic
   template. If this user never asked for source verification, don't invent a "verify before asserting"
   rule for them — include only what their own conversation actually supports, plus obviously-universal
   good practice (don't ask a clarifying question when a sensible default exists, scale depth to the
   question).
3. **What NOT to do** — the failure modes to avoid for this person specifically (e.g. if they've shown
   they dislike padding, say so explicitly; if they've shown they want maximal depth every time, note
   that instead of a generic "don't over-answer" line).

## Step 3 — Ask where to save it

This is a real decision for the user to make, not a default to guess:

- **User-level** (`~/.claude/skills/<name>/SKILL.md`) — applies across every project they open. Use
  this if their pattern is about how they generally like being taught/answered, independent of subject
  matter.
- **Project-level** (`.claude/skills/<name>/SKILL.md` in the current repo) — applies only here. Use this
  if the pattern is clearly specific to this codebase/domain rather than a general preference.

Ask which scope they want (a short direct question is fine — this is exactly the kind of choice that's
genuinely theirs to make and not inferable). Also ask what they'd like the skill named — suggest one
based on the theme you found (e.g. `learn-mode`, `concise-mode`, `rigorous-mode`) but let them override.

## Step 4 — Write the skill file

Use the same structural shape as a learn-mode skill (frontmatter `name` + `description`, then sections
for the recurring questions it replaces, how to answer instead, and what not to do) — but with content
entirely derived from Step 1-2's analysis of *this* user, not copied from any other skill.

Write the description so the skill auto-triggers broadly on substantive questions from this user (most
learn-mode-style skills want to fire on "every substantive question," not just when explicitly named —
say so in the description if that matches what the user wants).

## Step 5 — Offer a backing memory entry (optional, ask first)

A memory entry in the current project's memory folder makes the preference recalled automatically even
before skill-matching kicks in, mirroring how the original learn-mode skill was backed. Offer this;
don't do it unasked, since not every user wants a persistent per-project note about how they like being
answered.

## Step 6 — Report back plainly

Tell the user exactly what pattern you detected (in their own terms, not abstractly), where the file
was saved, and how to check or edit it. If you had to guess at anything due to thin conversation signal,
say so explicitly rather than presenting a low-confidence skill as a confident one.
