---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree **one question at a time**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. At each step, pick the single highest-value frontier question and put that one question to the user; then **wait for the reply** before asking anything else. Never batch multiple questions into one message or one `ask` call.

Put each question to the user as **chat prose**. For every question:

- give the **context** the user needs in order to answer it;
- explain **why it matters**, what the answer changes downstream;
- when the question has options, label them **Option A**, **Option B**, and so on, and compare their **pros**, **cons**, **ripple effects**, and **potential unintended consequences**;
- state your own **recommendation**, and explain why you chose it.

The options themselves go to the user through omp's `ask` tool, one `ask` call per question, after the prose, labels matching the prose, the recommended option marked. The prose carries the reasoning and the recommendation; `ask` carries the choices. A question with no enumerable options is put in prose alone, and the user answers in prose.

Each answer reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next single question. A question whose answer depends on another question still open belongs to a _later_ step, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; questions independent of the running exploration continue in the one-at-a-time rhythm without waiting for that exploration. When an investigation needs a script, follow the standing convention: Python, safe, robust (every probe failure-tolerant), strictly read-only apart from its report file, the report living next to the script with the same filename and a different extension.

The _decisions_ are the user's: put each to them one at a time and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
