# adaptive-teacher

A [Claude Code](https://claude.com/claude-code) skill for being taught —
anything. It answers your clarifying questions in the style the
learning-science literature supports, treats every question you ask as a
datum about what you know, and maintains a persistent learner model so the
teaching adapts to you and improves across sessions. Domain-general: the
material can be mathematics, Lean, programming, or anything else.

Refactored out of the
[mathlib-definition](https://github.com/homeowmorphism/mathlib-definition)
skill (2026-08-01), where the protocol was originally distilled; that skill
now handles only the definition-design workflow and hands conversation-side
teaching to this one.

## When it triggers

Use it (or let Claude pick it up from the skill description) when you:

- ask a clarifying or conceptual question ("what does X mean?", "why does
  this work?", "what's the difference between X and Y?");
- ask to be taught a topic, walked through something step-by-step, or given
  exercises;
- ask for an explanation in simpler terms;
- follow up on any explanation, including one produced by another skill.

## How it works

1. **Read the learner model first.** `learner/model.md` records what you
   already know (skipped or compressed), your active gaps, recurring
   question patterns (preempted), which explanation styles land, and a
   spaced retrieval queue (at most one due prompt per session, always
   skippable).
2. **Diagnose, then answer.** Each question is answered by an eight-rule,
   evidence-based answer-style protocol: answer first, diagnosis out loud,
   refutation shape for false premises, per-axis calibration to the learner
   model, mechanism demonstrated with checkable artifacts rather than
   asserted, contingent depth, sparse signaling, one generative hook. Every
   rule is pinned to verified citations in
   [references/adaptive-teaching.md](references/adaptive-teaching.md).
3. **Log and consolidate.** The question is logged verbatim with a
   diagnosis; the log consolidates into the model before the session ends,
   so the next session starts smarter.

## Repository layout

```
SKILL.md                      entry point: the loop, teaching moves, hard rules
references/
  adaptive-teaching.md        the full protocol: two-tier design, file
                              schemas, the learner loop's learning-science
                              grounding, the eight-rule answer-style
                              protocol, honest limits, and two verified
                              bibliographies
learner/                      personal, gitignored: the verbatim question
                              log and the consolidated learner model the
                              loop maintains for this installation's user
```

The `learner/` tier is deliberately split from the tracked files: what the
skill learns *about you* (your question history, what you already know,
which explanation styles land) stays local and private; what your questions
reveal about the skill's own text (a missing prerequisite, an explanation
anyone would stumble on) gets fixed in `references/` and committed, so the
skill itself improves too.

## Installation

As a personal skill (available in all projects), clone into
`~/.claude/skills/adaptive-teacher`. As a project skill, clone into
`.claude/skills/adaptive-teacher` inside the project instead. Claude Code
discovers the skill from the `SKILL.md` frontmatter; invoke it explicitly
with `/adaptive-teacher`, or just ask a question and let it trigger from
the description.

## License

[Apache License 2.0](LICENSE.md): free to use, modify, and share, for any
purpose, with an explicit patent grant. Keep the license and attribution
notices with any redistribution.
