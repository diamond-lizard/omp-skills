# omp-skills

A pack of six skills for the omp (oh my pi) harness, installed as one self-managed directory at `~/.omp/agent/skills/omp-skills/`.

## What this is

Six skills:

- **wayfinder**: plan a huge chunk of work (more than one agent session can hold) as a shared map of decision tickets on your issue tracker, and resolve them one at a time until the way to the destination is clear. Requires rhizome-mcp.
- **grilling**: grill the user relentlessly about a plan, decision, or idea; stress-test their thinking.
- **domain-modeling**: build and sharpen a project's domain model: shared terminology, a CONTEXT.md, ADRs.
- **grill-with-docs**: a grilling interview whose resolutions are captured as they settle — glossary entries and ADRs written during the conversation, not after. Runs grilling and domain-modeling together; invoke it by name when you want capture, plain grilling when you don't.
- **research**: investigate a question against high-trust primary sources and capture the findings as a Markdown file in the repo.
- **prototype**: build a throwaway prototype to answer a design question.

wayfinder requires rhizome-mcp (https://github.com/Odrin/rhizome-mcp); the other five do not.

## Install

The install, update, and uninstall procedure is in [docs/how-to-install-skills-in-omp.md](docs/how-to-install-skills-in-omp.md), written to be executed by an LLM inside an omp session: point your agent at this repo (a local directory path or a git URL) and ask it to install the six skills, using `omp-skills` as the pack name.

## Prerequisites

- omp installed.
- For wayfinder only: rhizome-mcp installed and initialized in the repo you wayfind in (see https://github.com/Odrin/rhizome-mcp), and registered as an omp MCP server (user-level `~/.omp/agent/mcp.json` or project-level `<repo>/.omp/mcp.json`). The wayfinder skill checks availability at run time and instructs the user if it is missing.

## Invocation

In omp, invoke a skill by name: `/skill:<name>` (for example `/skill:wayfinder`). Skills are installed to `~/.omp/agent/skills/omp-skills/`. Note that wayfinder and grill-with-docs set `disable-model-invocation: true`, which hides them from the model's skill list — the model will not pick them up on its own — but not from yours: they still appear in the `/skill:` menu and are invoked explicitly, like any other skill.

## Credits

Many of these skills were inspired by or adopted from other sources.  See [docs/credits.md](docs/credits.md) for details.
