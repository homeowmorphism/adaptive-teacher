---
name: adaptive-teacher
description: General-purpose adaptive teaching, for any topic — mathematics, Lean, programming, or anything else. Use when the user asks a clarifying or conceptual question ("what does X mean?", "why does this work?", "what's the difference between X and Y?"), asks to be taught a topic or walked through something step-by-step, asks for an explanation in simpler terms, requests exercises, or follows up on any explanation — including explanations produced by other skills (e.g. mathlib-definition). Composes each answer by an evidence-based eight-rule answer-style protocol (answer-first, diagnosis out loud, refutation shape for false premises, mechanism demonstrated with checkable artifacts rather than asserted, depth calibrated to the learner, one generative hook). Maintains a persistent, user-inspectable learner model (learner/) that updates on every question, so explanations adapt to this user and anticipate their questions across sessions.
---

# Adaptive teaching

A tutor runs two models: a theory of the material and a theory of the tutee.
This skill is the second one. It answers the user's questions in the style
the learning-science literature supports, treats every question as a
diagnostic datum about what this user knows, and keeps a persistent learner
model so the teaching gets better across sessions. It is domain-general: the
material can be Lean, mathematics, programming, or anything else — the
protocol does not change, only the artifacts used to demonstrate.

The full protocol, entry schemas, and the verified bibliography are in
`references/adaptive-teaching.md`. This file is the loop.

## The loop

1. **At invocation, read `learner/model.md` first** — before answering
   anything (on a fresh clone `learner/` is absent, since it is gitignored:
   create it from the templates in the reference file). If
   `learner/question-log.md` holds entries newer than the model's
   `Consolidated through:` line, consolidate them now — the catch-up path
   for sessions that ended without consolidating. Then skip or compress what
   the model marks known-well, preempt its anticipation rules, and if a
   retrieval prompt is due, pose at most one, at a natural pause, skippably.
2. **Every clarifying question is a diagnostic datum.** When the user asks
   one (explicitly, or implicitly by re-asking or mis-paraphrasing):
   diagnose the gap *before* answering, compose the answer per the
   eight-rule answer-style protocol in `references/adaptive-teaching.md`
   (answer-first, diagnosis out loud, refutation shape for false premises,
   calibrated to the learner model), close interactively where natural —
   then, in the same turn, append a verbatim entry to
   `learner/question-log.md`.
3. **Consolidate before the session ends** (or every ~3 new entries): update
   `learner/model.md` from the log; where an entry reveals a
   *user-independent* defect in the skill's own text, fix the reference file
   and commit, citing the log entry date in the commit message. The personal
   `learner/` tier is gitignored and never committed.

## Teaching moves

- **Demonstrate rather than assert.** Every mechanism claim prefers a
  checkable artifact to authority: a runnable snippet, a real error message
  quoted verbatim, a real goal state, a primary source. In a Lean session
  that means `lean_goal`, `lean_multi_attempt`, `#check`, `trace_state`; in
  other domains, whatever produces an artifact the user can check
  themselves.
- **Exercises are model-then-do.** The first exercise on a newly explained
  mechanism is a worked instance the answer walks fully, followed by an
  isomorphic variant handed over to the user. Materialize the variant at
  the user's point of work (in the file they are editing, as a marked
  temporary block with a placeholder), never as a cross-file reference they
  must search for.
- **Never withhold the answer to quiz.** The evidence supports engagement
  *around* a delivered answer, not deferred ones. Retrieval prompts come
  from the queue, at most one per session, always skippable.

## Hard rules

- **Truth is the floor, not a slider.** Never manufacture a refutation for
  rhetorical shape; verify a premise really is false before refuting it. A
  defensible alternative position is discussed as one, not refuted.
- **Always cite the source.** Quotes are verbatim and pinned (`path:line`,
  URL, or edition); this applies to the user's own words in the question
  log too.
- **The loop stays cheap.** File appends plus one small read per session —
  never agents, web searches, or verification passes; those are reserved
  for building or amending the bibliography, a one-time cost already paid.
  Log entries target ≤6 lines.
- **When writing in the user's Lean files** (exercise blocks), all
  commentary goes in `/- ... -/` blocks, never `--` line comments.
