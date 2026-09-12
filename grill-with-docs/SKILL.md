---
name: grill-with-docs
description: A relentless interview to sharpen a plan or design, which also creates docs (ADRs and glossary) as we go.
disable-model-invocation: true
---

A composite entry point, not a methodology: it owns no interview technique of its own. It runs the grilling interview and the domain-modeling recording discipline together, so the conversation's resolutions are written down as they are settled rather than left in chat scrollback.

Read the skills via `skill://grilling` and `skill://domain-modeling` and follow them both:

- `grilling` drives the conversation: the design tree, one question at a time through omp's `ask` tool, waiting for each reply.
- `domain-modeling` captures: the moment a term resolves, update `CONTEXT.md` inline (never batched); when a decision passes the ADR criteria, offer the ADR.

Pick this skill instead of plain `grilling` when the user wants the interview's resolutions captured in the repo's docs. User invocation decides: `/skill:grill-with-docs` for the composite, `/skill:grilling` for the interview alone.
