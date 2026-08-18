---
name: learn-mode-builder
description: Use when someone asks to build, generate, or create a personalized "learn-mode"-style answering skill for THEMSELVES — e.g. "build me a learn-mode skill", "generate a personalized answering skill based on this conversation", "make a skill like that other user's learn-mode but for me". Analyzes the CURRENT conversation for the invoking user's own question-asking and understanding patterns — including whether those patterns vary by category of question within this conversation (e.g. architecture vs. debugging vs. conceptual), and whether this conversation represents a genuinely separate context from any learn-mode-style skill the user already has (e.g. codebase-understanding vs. learning an unrelated technical skill) — then writes a fresh, personalized skill tailored to them, named and scoped to avoid colliding with any existing one. Never copies another user's learn-mode verbatim — every run re-derives the pattern from scratch.
---

# Learn-Mode Builder

This skill produces a **new, personalized** `learn-mode`-style skill for whoever is running it right
now, built entirely from patterns observed in *their own* current conversation. It does not template
or copy any specific prior learn-mode skill's content — the source material each time is the live
transcript, not a fixed example.

## Step 0 — Check whether this conversation is a new context, not just a new pattern

Before analyzing anything, check whether the user already has one or more learn-mode-style skills
installed (`~/.claude/skills/*/SKILL.md` and, if relevant, `.claude/skills/*/SKILL.md` in the current
project — look for skills whose description matches the "personalized answering pattern" shape this
builder produces).

If one or more already exist, decide whether *this* conversation belongs inside one of them or is a
genuinely separate context:

- **Same context, more signal** — this conversation is more of the same domain/scope an existing skill
  already covers (e.g. you already have a codebase-understanding skill and this conversation is more
  codebase-understanding, maybe on a different repo but the same kind of activity). Treat this as
  refining that existing skill's pattern, not minting a new one — ask the user whether they want the
  existing skill updated instead of a new one created.
- **Genuinely separate context** — this conversation differs from the existing skill(s) in something
  *structural*, not just topic:
  - **Different verification substrate** — one is anchored to reading actual source/config in a real
    codebase; another is anchored to general knowledge, docs, or concepts with no repo to check against.
    "Verify before asserting" means a different action in each, not just a different weight.
  - **Different natural scope** — one is inherently tied to a specific project (belongs project-level);
    the other is portable across any project or none (belongs user-level).
  - **Rarely blend in one question** — the two kinds of conversation don't typically co-occur inside a
    single question (unlike within-conversation categories, e.g. architecture vs. debugging on the same
    codebase, which routinely do — see Step 2).

  Only when a difference like this is present should this run produce a **new, separate** skill rather
  than folding into an existing one. If unsure, ask the user directly rather than guessing — this is
  exactly the kind of judgment call that's genuinely theirs to make.

This step is about *whether to create a new skill at all* and is separate from Step 2, which is about
*how one skill's internal rules should be organized* once you've decided to (or continue to) write it.

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

**Does their pattern vary by category of question, within this conversation?** Compare how they reacted
across different kinds of questions actually present in this conversation — e.g. did they demand
source-verification for code/implementation questions but not for conceptual/definitional ones? Did
they want a design verdict only on architecture-style questions but just want steps for tooling/config
questions? Check this explicitly rather than averaging every reaction into one flat pattern — a user
who verifies-hard on code but wants quick plain answers on concepts is not "moderately rigorous,"
they're rigorous *conditionally*, and flattening that loses real signal.

**Honesty safeguard — do not fabricate a pattern from a thin conversation.** If the conversation so far
is too short (a handful of exchanges, or all on unrelated one-off topics) to confidently identify a real
recurring pattern, say so plainly instead of inventing one. In that case, either:
- ask the user directly what they'd want emphasized (a couple of concrete questions is fine — this is
  exactly the kind of case-specific, otherwise-unknowable decision worth asking about), or
- offer to build a lighter, more general-purpose starting skill now and refine it after a few more
  real exchanges.

**Category safeguard — don't invent categories from a single-category conversation.** If everything
asked so far has been one kind of question (e.g. all code-review questions), there's no evidence their
pattern varies by category — build flat (see Step 2). Only treat the pattern as categorized when the
conversation contains 2+ distinct question categories *and* the user's expected reaction actually
differed between them. A conversation that spans categories but gets the *same* reaction every time
(e.g. they always want file:line citations no matter the topic) is still a flat pattern — categorize
based on divergent behavior, not divergent topics.

Never assume the user's pattern resembles any other specific person's learn-mode skill. Every user is
different — some want terse answers, some want maximal rigor, some want reassurance rather than
critique. Derive the pattern from evidence in front of you.

## Step 2 — Synthesize the pattern into the right shape

First, decide the skill's shape based on Step 1's findings:

- **Flat** (the default) — one rule list applying uniformly, used when no real category-varying signal
  was found.
- **Categorized** — used only when Step 1 found genuine category-varying signal *within this
  conversation's single context*. Instead of one flat rule list, write a routing layer on top of the
  same rules: 2-4 categories actually observed in this conversation, named from what was actually asked
  (e.g. "debugging questions" vs. "library-choice questions" — not a generic template like "technical
  vs. non-technical"), each noting which rules dominate or get skipped for that category. Keep this as
  *one skill file* with an internal routing section — never split it into multiple separate skill files
  for within-context categories. Separate skills would compete on Claude's skill-selection matching (a
  coarse, description-level match) for every question, and most real within-context questions blend
  categories rather than falling cleanly into one — a routing decision made *inside* one skill's
  instructions, with the full question in view, is far more reliable than picking between several
  similarly-triggered skills upfront. It would also duplicate every category-independent rule (e.g.
  "verify before asserting") across files, so a future preference change has to be applied in every copy
  instead of once.

(If Step 0 determined this conversation is a genuinely *separate context* from an existing skill, that
split already happened at the skill level — don't also try to re-litigate it here as a category choice.
This step only organizes rules *within* the one skill this run is producing.)

Then write, in whichever shape was chosen:

1. **The recurring counter-questions this replaces** — 4-6 concrete follow-ups this specific user tends
   to ask, phrased in their own words where possible (quote or closely paraphrase actual things they
   said in this conversation). If categorized, group these by category where the follow-up itself
   differed.
2. **How to answer instead** — a numbered list of concrete behaviors that front-load those follow-ups
   into the first response. Ground each instruction in what was actually observed, not a generic
   template. If this user never asked for source verification, don't invent a "verify before asserting"
   rule for them — include only what their own conversation actually supports, plus obviously-universal
   good practice (don't ask a clarifying question when a sensible default exists, scale depth to the
   question). If categorized, this is where the "calibrate by category" routing section goes — sitting
   on top of the shared rules, not replacing them.
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
based on the theme you found, but let them override.

**If Step 0 found existing learn-mode-style skill(s), name and scope this one to avoid collision:**
- Name after this conversation's own domain, not a generic label — e.g. `codebase-learn-mode` (paired
  with something like `concept-learn-mode` for a general-knowledge one), not two skills both loosely
  called "learn-mode." A reader should be able to tell the two apart by name alone.
- Prefer the scope that matches the structural difference found in Step 0 — a codebase-anchored context
  usually belongs project-level (or user-level only if it's meant to apply across many repos the same
  way); a portable, general-knowledge context usually belongs user-level.

Note: this scope question (project vs. user-level) is about *where the skill applies*, and is separate
from the category-routing decision in Step 2 (which is about *how the skill behaves* once it applies),
and separate again from the Step 0 decision (whether a *new* skill should exist at all).

## Step 4 — Write the skill file

Use the same structural shape as a learn-mode skill (frontmatter `name` + `description`, then sections
for the recurring questions it replaces, how to answer instead, and what not to do) — but with content
entirely derived from Step 1-2's analysis of *this* user, not copied from any other skill. If the
pattern was categorized, the "how to answer instead" section includes the routing layer from Step 2,
still inside this single file.

Write the description so the skill auto-triggers broadly on substantive questions from this user (most
learn-mode-style skills want to fire on "every substantive question," not just when explicitly named —
say so in the description if that matches what the user wants). If categorized, the description should
still describe one broadly-triggering skill — the categories are an internal calibration detail, not
separate trigger conditions for separate skills.

**If this skill coexists with another learn-mode-style skill (per Step 0), make the description
explicitly exclude the other's territory**, not just carry a distinct name. Skill-selection reasons over
description text, so state the boundary in words — e.g. "...for questions about this specific
codebase/repo, not general conceptual learning" on one, and "...for general technical/conceptual
learning not anchored to a specific codebase's actual source" on the other. Note plainly to the user
that some genuinely ambiguous questions (e.g. a general-concept question asked only because of
something just seen in the code) may still route to either skill somewhat unpredictably — state a
reasonable tiebreaker (e.g. "if the question references real file/function names, prefer the codebase
skill") rather than promising perfect separation.

## Step 5 — Offer a backing memory entry (optional, ask first)

A memory entry in the current project's memory folder makes the preference recalled automatically even
before skill-matching kicks in, mirroring how the original learn-mode skill was backed. Offer this;
don't do it unasked, since not every user wants a persistent per-project note about how they like being
answered.

## Step 6 — Report back plainly

Tell the user exactly what pattern you detected (in their own terms, not abstractly), whether the
resulting skill is flat or categorized and what evidence supported that call, whether it was created as
a new skill or folded into an existing one (and why, per Step 0), where the file was saved, and how to
check or edit it. If you had to guess at anything due to thin conversation signal, say so explicitly
rather than presenting a low-confidence skill as a confident one.
