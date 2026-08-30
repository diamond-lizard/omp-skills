# omp-skills

A pack of five skills for the omp (oh my pi) harness, installed as one self-managed directory at `~/.omp/agent/skills/omp-skills/`.

This README is written to be executed by an LLM inside an omp session: point your agent at this repo (a local directory path or a git URL) and ask it to install. The install is a copy.

## What this is

Five skills:

- **wayfinder**: plan a huge chunk of work (more than one agent session can hold) as a shared map of decision tickets on your issue tracker, and resolve them one at a time until the way to the destination is clear. Requires rhizome-mcp.
- **grilling**: grill the user relentlessly about a plan, decision, or idea; stress-test their thinking.
- **domain-modeling**: build and sharpen a project's domain model: shared terminology, a CONTEXT.md, ADRs.
- **research**: investigate a question against high-trust primary sources and capture the findings as a Markdown file in the repo.
- **prototype**: build a throwaway prototype to answer a design question.

wayfinder requires rhizome-mcp (https://github.com/Odrin/rhizome-mcp); the other four do not.

## Install

The procedure takes a single input, **SOURCE**: a local directory path to this repo, or a git clone URL for it. Treat every step as conditional on current state; running the whole procedure again is safe and yields the same result (idempotent).

1. **Resolve SOURCE.**
   - If SOURCE is a git URL: shallow-clone it into a temporary directory (`git clone --depth 1 <SOURCE> <tmpdir>`) and use that as the source tree.
   - If SOURCE is a local directory path: use it in place.
   The five skill directories sit at the top level of the source tree: `wayfinder/`, `grilling/`, `domain-modeling/`, `research/`, `prototype/`.
2. **Create the managed directory** `~/.omp/agent/skills/omp-skills/` if it does not exist.
3. **Copy the five skill directories** from the source tree into the managed directory, silently overwriting an existing directory of the same name.
4. **Reconcile deletions.** Remove any directory under the managed directory that is not one of the five skills present in the source tree. This is a reconcile (copy in, overwrite, delete stale), not a blind copy. The managed directory is wholly owned by this procedure, so reconciling inside it is always safe; never touch `~/.omp/agent/skills/` outside the managed directory.
5. **Register the settings entry.** Read `~/.omp/agent/config.yml` (the omp user settings file for the active profile; create it if missing) and ensure the `skills.customDirectories` setting contains `~/.omp/agent/skills/omp-skills`. Merge into any existing `skills` block; never remove or reorder other entries. Check before writing: if the entry is already present, change nothing. The tilde form is fine (omp expands tilde when scanning custom directories).
6. **Verify.** Each of the five directories under the managed directory contains a `SKILL.md`; the settings entry is present. Report the installed skill names (wayfinder, grilling, domain-modeling, research, prototype) and remind the user that omp discovers skills at session start, so they should start a fresh omp session (or restart the current one) for the skills to appear.

## Update

Identical procedure to install: full reconciliation under the managed directory (add new skills, overwrite changed ones, delete absent ones), then re-verify that `skills.customDirectories` still contains `~/.omp/agent/skills/omp-skills` and restore it if lost (merging as in install step 5). A fresh omp session is needed after any update.

## Uninstall

1. Remove the managed directory `~/.omp/agent/skills/omp-skills/`.
2. Remove the `skills.customDirectories` entry for it from `~/.omp/agent/config.yml`, leaving every other entry intact.

## Prerequisites

- omp installed.
- For wayfinder only: rhizome-mcp installed and initialized in the repo you wayfind in (see https://github.com/Odrin/rhizome-mcp), and registered as an omp MCP server (user-level `~/.omp/agent/mcp.json` or project-level `<repo>/.omp/mcp.json`). The wayfinder skill checks availability at run time and instructs the user if it is missing.

## Invocation

In omp, invoke a skill by name: `/skill:<name>` (for example `/skill:wayfinder`). Skills are installed to `~/.omp/agent/skills/omp-skills/`.

## Credits

These skills are inspired by and adapted from [mattpocock/skills](https://github.com/mattpocock/skills), reworked for the omp harness and the rhizome-mcp issue tracker.
