# Post-mortem

![Post-mortem: learn from failures, record recovery attempts, verify fixes, and retain lessons in the project memory bank](assets/Post-Mortem_%20From%20Failure%20to%20Learning.png)

An automatic Codex skill that helps an agent learn from trial and error within
a project. It records unexpected failures and every recovery attempt, then
consults that history before trying again.

## Installation

With the Codex CLI available:

```sh
codex plugin marketplace add gonzoinc/post-mortem
codex plugin add post-mortem@post-mortem
```

For a local checkout, replace `gonzoinc/post-mortem` in the first command with
its folder path. The package uses the supported [Codex marketplace format](https://developers.openai.com/plugins/build/plugins).

The same skill is included in [Gonzo Skills](https://github.com/gonzoinc/skills).
Install it through one package to avoid duplicate copies. This standalone
package needs none of that collection's other skills, MCP servers or accounts.

## How it works

1. An attempted action unexpectedly fails or an approach proves wrong.
2. The agent finds the project's memory bank and reads relevant past incidents.
3. It records the failure before recovery and keeps a chronological log of
   every attempt, hypothesis, actual result and supporting evidence.
4. A previously failed approach can be repeated only with recorded new evidence
   or changed conditions that justify the retry.
5. The incident is resolved only after checking the original goal. Otherwise it
   stays open or blocked so another session can continue from the evidence.

The history lives in the established project memory bank's `post-mortem.md`.
When no memory bank exists, the fallback is:

```text
<project>/.codex/memory-bank/post-mortem.md
```

No history file is created until an incident needs recording. Each project
keeps its own history; this skill does not write a shared global memory bank.
Existing managed-memory restrictions take precedence.

## What gets recorded

Each incident contains the goal, context, failure, confirmed cause or labeled
hypothesis, all recovery attempts, verification and a prevention lesson.
Resolved incidents can be reopened when they recur, retaining earlier evidence.
Credentials and sensitive payloads are excluded.

Deliberately failing tests and other expected negative outcomes are not incidents.
Successful steps that help resolve a real incident are recorded in that incident.
Routine recovery stays quiet; the agent asks one question when your input is
needed or it cannot make progress.

## Automatic activation

[`agents/openai.yaml`](skills/post-mortem/agents/openai.yaml) sets
`allow_implicit_invocation: true`. The host/model may select the skill when an
unexpected failure occurs, without you typing a command. You can also select
`/post-mortem` explicitly from Codex's skill menu.

A skill is not a background service or an enforced callback after every failed
command. Automatic discovery does not guarantee that every failure will be
intercepted. Enforced interception would require separate host integration.
The file provides persistent context when read; it does not retrain the model.

## Skill instructions

Read the [complete workflow](skills/post-mortem/SKILL.md).

Metadata checks and sampled recovery decisions passed. Live history-writing,
cross-session persistence and concurrent updates still require end-to-end verification.

## License

Original skill by gonzoinc, released under the [MIT license](LICENSE).
