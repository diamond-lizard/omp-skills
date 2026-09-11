---
name: research
description: Investigate a question against high-trust primary sources and capture the findings as a Markdown file in the repo. Use when the user wants a topic researched, docs or API facts gathered, or reading legwork delegated to a background agent.
---

Spin up a **background agent** (an omp `task` subagent or workpool item) to do the research, so you keep working while it reads.

Its job:

1. Investigate the question against **primary sources** (official docs, source code, specs, first-party APIs), not a secondary write-up of them. Follow every claim back to the source that owns it.
2. Write the findings to a single Markdown file, citing each claim's source.
3. Save it where the repo already keeps such notes; match the existing convention, and if there is none, put it somewhere sensible and say where.

When the investigation needs a script, follow the standing convention: Python, safe, robust (every probe failure-tolerant), strictly read-only apart from its report file, the report living next to the script with the same filename and a different extension. Never download release artifacts or large model files (release tarballs, GGUFs, toolkits), not even into scratch space: if host-side facts about such an artifact are needed, either ask the user to download and run it, or hand them a read-only investigative script to run on the target host.

When researching inside a wayfinder ticket, the findings file is the durable artifact (committed per that skill's charting step); the ticket gets a context-pointer comment with the path, and rhizome comments are a convenience copy for searchability — the file, not the comment, is the system of record.
