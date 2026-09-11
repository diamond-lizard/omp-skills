# Rhizome tracker operations

This document is wayfinder's tracker doc. It defines every tracker operation a wayfinding session performs, on rhizome-mcp only. There is no other tracker and no fallback.

## Availability gate

Run this gate before any other wayfinder action. It is an unconditional precondition.

**Step 1: tools present?** Check whether any `mcp__rhizome_mcp_*` tools are available in this session. If they are, go to step 2. If none are available, fail fast: no degraded mode, no partial work. Ask the user whether rhizome-mcp is installed on this machine.

- If it is not installed: point the user at the rhizome-mcp home page (https://github.com/Odrin/rhizome-mcp) for the install options (an npx trial or the installer). After install, have `rhizome-mcp init` run in this repo, register the server with omp, restart the session, and retry the gate.
- If it is installed: register it with omp. Registration is an MCP server entry, either user-level in `~/.omp/agent/mcp.json` or project-level in `<repo>/.omp/mcp.json`: a stdio server running `rhizome-mcp serve` with the repo pinned via `--project-root <repo absolute root>` where sensible. Then start a fresh session and retry the gate.

**Step 2: project reachable?** (only when the tools exist) Call `open_project` with this repo's absolute root. Routed opens are existing-only: they never create a database or run init/migrations. On `PROJECT_NOT_FOUND`, have `rhizome-mcp init` run in this repo, then retry `open_project` in the same session. When the open succeeds, the gate passes.

## Orientation

Call `open_project` with the repo's absolute root. Retain the returned `project_ref` and pass it to every project-scoped call that follows: rhizome-mcp routing is stateless, so each call must carry the project_ref.

## General operations

- **Create**: `create_issue`. Pass labels with `create_missing_labels: true` so wayfinder labels exist on first use.
- **Read**: `get_issue` (by ULID or ISSUE-N display id) for the current record; `get_issue_activity` for the unified, newest-first timeline of work and artifacts on an issue.
- **Comment**: `add_comment` is append-only. Never rewrite history; add a new comment.
- **Patch**: `update_issue` with `expected_version` for optimistic concurrency. On a version conflict, refetch with `get_issue`, reconcile your change against the newer record, and retry with the fresh version.
- **Multi-issue charts**: `validate_issue_plan` then `apply_issue_plan`: one atomic batch (up to 50 issues, 100 relations, 20 decisions).
- **Attribution**: none. Do not create agent sessions and do not mention session handles; NULL attribution is the standing mode.

## Wayfinding operations

### Map

One epic, label `wayfinder:map`, whose description holds the map body: `## Destination`, `## Notes`, `## Decisions so far`, `## Not yet specified`, `## Out of scope`.

### Child ticket

A `task` or `bug` with `parent_issue_id` set to the map, created with status `ready` (rhizome's open means not-yet-executable, which is wrong for charted tickets). Label each with `wayfinder:<type>`: `research`, `prototype`, `grilling`, or `task`.

### Blocking

`manage_issue_relation` with relation_type `blocks` (source blocks target). Keep the graph acyclic. When charting, wire every blocking edge in the same atomic batch as ticket creation (`validate_issue_plan` then `apply_issue_plan`).

### Frontier

`list_issues` with `parent_issue_id: <map>` and `is_claimable: true`, or `get_planning_graph` rooted at the map.

### Claim

`claim_issue` with a lease of up to 3600 seconds is the session's first write on a ticket: claim before any work, so concurrent sessions skip it. When the user's instruction to work on an issue arrives, treat it as standing authorization for any legitimate pre-claim transitions (e.g. `open`→`ready`) via supported tools — no separate confirmation round-trip; the state check and claim-before-content order still apply.

- Renew the lease on every wake-up (human reply, subagent return, user answer) via `renew_attempt`.
- Append a progress note via `save_attempt_note` after every significant finding; checkpoints seed recovery for a successor attempt.
- On renew failure: read `get_work_context` (its attempt history). If the ticket was resolved meanwhile, discard your work quietly; otherwise yield with a handoff note and stop.
- A lapsed lease makes `finish_attempt` fail loudly, so conflicting resolutions cannot both land.
- Do not retry a suspected-successful claim with the same idempotency key: a retry rotates the lease token.

### Resolve

Post the answer with `add_comment`, then `finish_attempt` with outcome `completed` and `target_issue_status` `done` (the only path to done; direct `update_issue` cannot set it), then append one gist line to the map's Decisions so far via `update_issue` on the map (CAS with `expected_version`; on conflict, refetch, re-append, retry).

### Out of scope

`add_comment` with why, then `update_issue` with status `cancelled` (never `archive_issue`: archiving hides the issue and breaks the Out-of-scope line's reference to it), then add the Out-of-scope line on the map.

### Context pointers

Assets created while resolving a ticket are recorded as a comment on the resolving ticket: prototype artifacts, research notes — a repo file path, per the findings convention — and specs; branch name, path, or URL, whichever points at the asset. Committing a findings file during planning touches the user's working repo: report the commit explicitly, and ask first when the destination branch or location is ambiguous.

## Map maintenance

- Every map edit goes through `update_issue` with `expected_version`; on conflict, refetch, re-apply, retry.
- An invalidating resolution edits the index (the map) and every affected ticket in the same session.
- Decisions so far is theme-structured under headings from day one: group gist lines under small theme headings rather than one flat list.
- When the map description exceeds 10,000 characters, the resolving session tidies Decisions so far with a per-line relevance test: merge overlapping lines into theme-grouped lines, drop lines about settled territory. Nothing durable is lost: closed tickets keep their resolution comments, and rhizome's event log preserves every dropped line. (Rhizome's description limit is 100,000 characters.)

## Prerequisite

rhizome-mcp must be initialized in the repo; the availability gate covers its absence. One-time initialization: run `rhizome-mcp init` in the repo root.
