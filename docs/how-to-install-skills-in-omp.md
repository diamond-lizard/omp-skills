# How to install skills in omp

A generic procedure for installing one or more skills into the omp (oh my pi) harness. The skills arrive as a source tree of skill directories and are installed as one self-managed directory under `~/.omp/agent/skills/`.

This document is written to be executed by an LLM inside an omp session: point your agent at a directory of skills and ask it to install them. The install is a copy. Treat every step as conditional on current state; running the whole procedure again is safe and yields the same result (idempotent).

## What omp expects

- A skill is a directory containing a `SKILL.md`. Discovery from custom directories is non-recursive and strictly one level deep: `<custom-dir>/<skill-name>/SKILL.md`. A `SKILL.md` nested deeper, or sitting directly in the custom directory itself, is not discovered.
- Discovery from custom directories requires a `description` in the `SKILL.md` frontmatter; a skill without one is silently skipped. The invocation name is the frontmatter `name` if present, otherwise the skill directory's name.
- omp discovers skills at session start by scanning the directories listed in the `skills.customDirectories` setting in `~/.omp/agent/config.yml` (the omp user settings file for the active profile).
- A skill is invoked in a session by name: `/skill:<name>` (slash commands for skills are enabled by default via `skills.enableSkillCommands`).
- A skill whose frontmatter sets `disable-model-invocation: true` is hidden from the model's skill list (the model will not pick it up on its own) but stays reachable via `/skill:<name>` and `skill://<name>`.

## Paths and profiles

Everywhere this document writes `~/.omp/agent`, read it as **the active profile's agent directory**:

- Default profile: exactly `~/.omp/agent`; a system that only ever uses the default profile has no `~/.omp/profiles/` directory, and none is needed.
- Named profile (`omp --profile <name>`): `~/.omp/profiles/<name>/agent/`. That profile reads only its own `config.yml`, so it does not see an install registered only in the default profile: run this procedure with the profile's base in place of `~/.omp/agent`, or register the same managed directory in the profile's `config.yml`.

## Inputs

- **SOURCE**: a local directory path to a tree of skills, or a git URL for one.
- **NAME** (optional): a name for the managed skill pack. Defaults to the source tree's name — the repository name for a git URL, the directory name for a local path — sanitized to lowercase-hyphenated form (for example, a source directory `my-skills` installs to `~/.omp/agent/skills/my-skills/`). One install owns exactly one managed directory.

## Install

1. **Resolve SOURCE.**
   - If SOURCE is a git URL: shallow-clone it into a temporary directory (`git clone --depth 1 <SOURCE> <tmpdir>`) and use that as the source tree.
   - If SOURCE is a local directory path: use it in place.
2. **Find the skills.** The skill directories are the top-level directories of the source tree that contain a `SKILL.md`; ignore everything else. If the source root itself contains a `SKILL.md`, the whole tree is a single skill, and the source root is that skill's directory (its name is the frontmatter `name`, else the source directory's basename).
3. **Create the managed directory** `~/.omp/agent/skills/<NAME>/` if it does not exist.
4. **Copy the skills.** For each skill directory found in step 2, copy it into the managed directory so it lands at `~/.omp/agent/skills/<NAME>/<skill-name>/`, silently overwriting an existing directory of the same name. For a single-skill source tree, this means copying the source root into `~/.omp/agent/skills/<NAME>/<skill-name>/` — never as `SKILL.md` directly inside the managed directory, which omp would not discover.
5. **Reconcile deletions.** Remove any directory under the managed directory that is not present in the source tree. This is a reconcile (copy in, overwrite, delete stale), not a blind copy. The managed directory is wholly owned by this procedure, so reconciling inside it is always safe; never touch anything outside it — in particular other directories under `~/.omp/agent/skills/` and other entries in `skills.customDirectories`.
6. **Register the settings entry.** Read `~/.omp/agent/config.yml` (create it if missing) and ensure the `skills.customDirectories` setting contains `~/.omp/agent/skills/<NAME>`. Merge into any existing `skills` block; never remove or reorder other entries. Check before writing: if the entry is already present, change nothing. The tilde form is fine (omp expands tilde when scanning custom directories).
7. **Verify.** Each skill sits at `~/.omp/agent/skills/<NAME>/<skill-name>/SKILL.md` with a `description` in its frontmatter; the settings entry is present. Report the installed skill names and remind the user that omp discovers skills at session start, so they should start a fresh omp session (or restart the current one) for the skills to appear.

## Update

Identical procedure to install: full reconciliation under the managed directory (add new skills, overwrite changed ones, delete absent ones), then re-verify that `skills.customDirectories` still contains `~/.omp/agent/skills/<NAME>` and restore it if lost (merging as in install step 6). A fresh omp session is needed after any update.

## Uninstall

1. Remove the managed directory `~/.omp/agent/skills/<NAME>/`.
2. Remove its entry from `skills.customDirectories` in `~/.omp/agent/config.yml`, leaving every other entry intact.

## Prerequisites

- omp installed.
- Individual skills may have their own prerequisites (an MCP server, a CLI tool, credentials). Each skill's `SKILL.md` is the authority on this; check it before relying on the skill. MCP servers are registered as omp MCP servers in the user-level `~/.omp/agent/mcp.json` or the project-level `<repo>/.omp/mcp.json`.

## Invocation

In omp, invoke a skill by name: `/skill:<name>`, where `<name>` is the skill's frontmatter `name`, or its directory name if the frontmatter omits it. Skills installed by this procedure live in `~/.omp/agent/skills/<NAME>/`.
