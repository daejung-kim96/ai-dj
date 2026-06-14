# AI Agent Harness

An AI agent harness is the system around a language model that helps it work reliably.

The model supplies reasoning, language, and pattern recognition. The harness supplies the working environment:

- instructions
- context selection
- tools
- memory
- workflows
- verification
- guardrails
- logs and reports

## Simple model

```text
LLM model
-> reads selected context
-> follows a skill workflow
-> uses tools
-> verifies output
-> records durable decisions
```

Without a harness, an agent tends to rely on whatever is currently in the conversation. With a harness, the agent has a repeatable way to find the right files, apply the right workflow, and check the result.

## Why this matters

Good AI output usually depends on three things:

1. The model.
2. The context given to the model.
3. The workflow used to turn context into an answer or change.

Most teams focus only on the model. ai-dj focuses on the second and third parts.

## Harness vs prompt

A prompt is one instruction.

A harness is a reusable operating system for many prompts.

```text
Prompt:
  "Build this feature."

Harness:
  "Classify the request, read these files, apply this skill, verify these checks,
  and record any decision that should survive the chat."
```

## ai-dj interpretation

In ai-dj:

- `AGENT.md` defines the overall operating loop.
- `skills/` defines reusable workflows.
- `docs/rules/` defines cross-project rules.
- `docs/concepts/` explains ideas for study.
- `project-profiles/` keeps project-specific context.
- `evals/` defines checks before work is considered done.
- `reports/` stores one-off outputs.

The goal is not to make an agent read everything. The goal is to make it read the right things at the right time.
