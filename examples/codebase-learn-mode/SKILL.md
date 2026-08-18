---
name: codebase-learn-mode
description: Use for every substantive question where this user is trying to understand a REAL, specific codebase or system by reading its actual source — architecture, "how does X work," edge cases, design critique, or a full end-to-end flow. Answers are source-verified against real files/functions, calibrated by question type (mechanism vs. plain-explanation vs. design-critique), with substantial findings persisted as saved docs and whole-system understanding delivered as a visual artifact rather than more prose. Does NOT cover general conceptual/technical learning untethered to a specific repo's actual source (e.g. "how does OAuth work in general," "what's a good caching strategy") — that's the separate `learn-mode` skill's territory. Tiebreaker when a question could go either way: if it references real file/function names, says "in this codebase/repo," or depends on reading actual source to answer correctly, prefer this skill.
---

# Codebase Learn Mode — Deep-Diving a Real Codebase the Way This User Actually Does It

Built from a long, source-anchored investigation of one real codebase (AI-SDLC): a pattern of
question → source-verified answer → occasional persisted write-up → periodic zoom-out into a visual
artifact. This skill front-loads that pattern so a "how does X work" question, a "why is it named this
way" question, and a "give me the whole picture" request each get answered in their own right shape
from the first response, instead of needing three follow-up turns to get there.

## The recurring counter-questions this replaces, by category

**Mechanism questions** ("how does X work," "where does Y come from," "does it actually do Z"):
- "But is that verified against the actual code, or are you assuming?"
- "What happens in [a crash / two things running at once / a failure] case?"
- "Give me a concrete edge case where this actually breaks."

**Definition / plain-explanation questions**:
- "Explain that in very simple, easy words."
- "What does [term/phrase] mean?"

**Design-critique questions** ("isn't this a limitation," "why is it built this way," "am I right that..."):
- "Isn't that a bad design / a real gap? Shouldn't it work differently?"
- "Why is it called/named that — isn't that a little misleading?"
- "So correct me if I'm wrong: [their own stated mental model]."

**Standing requests that show up across all three categories above**:
- "Create one .md file for this in [the project's docs folder]."
- "Organize/audit my docs and correct anything that's wrong now."
- "Give me one complete visual artifact / diagram of the whole thing so I understand it by heart."

## How to answer instead

1. **Ground every claim in the actual current source before asserting it.** For "how does X behave in
   this codebase" questions, read the real file/config directly (or search until you can) rather than
   answering from memory, a generic pattern, or an earlier turn's assumption. If something can't be
   verified, say so rather than stating it with confidence. This user has repeatedly caught stale or
   assumed claims on inspection — verifying is the default, not a step to skip when time-pressured.

2. **Calibrate depth and tone by what kind of question it actually is** — decide from the whole
   question, not a keyword match, since real questions often blend these:
   - **Mechanism questions** → verify hard: cite real file paths and function/field names, walk the
     actual call chain in the order it executes, and proactively raise the adjacent edge case (a crash
     mid-operation, two things happening concurrently, what happens on failure/retry) before being
     asked.
   - **Definition/plain-explanation questions** → skip the file-citation ceremony. Answer directly in
     plain language first, with a short concrete example if it helps, and keep it brief — only expand
     into mechanism if the term turns out to hide something worth unpacking.
   - **Design-critique questions** → give an actual opinionated verdict with reasoning, never neutral
     narration. When the user proposes their own mental model, grade it precisely: confirm exactly
     which parts hold, isolate exactly which part is wrong, and correct that part specifically.

3. **Correct plainly when new evidence contradicts an earlier claim** — yours or the user's, in this
   session or referenced from a past one. Say what changed and why, directly, rather than smoothing it
   over or letting it quietly stand uncorrected.

4. **Persist substantial findings as a saved file, not just chat text**, when asked or when a deep
   multi-turn investigation naturally concludes — into the project's established docs location. If a
   hand-maintained tracking file (a running findings/issues log the user edits themselves) already
   exists there, never auto-edit it; add new write-ups alongside it and reference it, don't rewrite it.

5. **When asked to audit or reorganize a body of prior written findings, re-verify each claim against
   the current source rather than trusting the earlier notes** — codebases move. Correct what's stale
   and say plainly what changed, including if the correction is to something you wrote earlier in the
   same body of docs.

6. **For a "give me the complete picture" / "understand this by heart" request, reach for one
   comprehensive visual or diagrammatic artifact instead of another wall of prose.** This is specifically
   for whole-system-structure or whole-end-to-end-flow requests — not a substitute for a normal answer
   to a single mechanism question.

7. **Don't ask a clarifying question when a sensible default exists.** Pick it, state the assumption in
   one line, and proceed. Reserve real clarifying questions for choices that are genuinely the user's to
   make and hard to undo (where to save something, what to name it, whether to touch a file they
   maintain by hand).

## What NOT to do

- Don't run the full source-verification/citation ceremony on a simple definitional question — that's
  what the plain-explanation calibration in rule 2 is for. Over-verifying "what does X mean" reads as
  padding, not rigor.
- Don't skip verification on a mechanism question just because it was touched on loosely earlier in the
  conversation — re-check rather than assume an earlier pass still holds, especially mid-investigation
  where understanding is still being refined turn to turn.
- Don't produce a large visual artifact for a narrow single-mechanism question — reserve that for
  genuine whole-system or whole-flow requests, per rule 6.
- Don't silently skip correcting a stale claim during an audit pass just because you wrote it earlier in
  the same conversation — hold your own prior claims to the same scrutiny as anything else being checked.
