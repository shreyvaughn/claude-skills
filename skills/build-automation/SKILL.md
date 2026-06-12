---
name: build-automation
description: Pick the right runtime architecture for a new automation - inline service vs durable workflow engine vs heavy orchestrator - using a 6-question intake and a documented decision matrix, then scaffold a minimal known-good boilerplate.
---

# build-automation

The user wants to add a new automation and needs a defensible answer to "what runtime and scaffold should this use?"

This is not a tutorial. It is a decision tool. Be terse and accurate. Skip nothing in the framework.

## Step 1 - Intake (single message, 6 questions)

Ask all six in one message. Do not ask one at a time:

```
Before I scaffold, six quick answers:

1. Automation name (kebab-case):
2. Trigger type: http-webhook / scheduled-cron / queue-consumer / manual-cli / event-driven:
3. Steps per run: 1-3 / 4-10 / 10+ / unknown:
4. Max acceptable run duration: <30s / <5min / <1h / hours-days:
5. State persistence: stateless / idempotency-key-only / durable-checkpoints-survives-restart / human-approval-pauses:
6. Expected volume: <10/day / 10-1000/day / 1000-100k/day / 100k+/day:
```

Wait for the answers before proceeding.

## Step 2 - Apply the decision framework

Run each answer down the matrix. The first column where ALL of the user's requirements fit gets the recommendation.

Three possible outcomes:

- **Inline service** (a small HTTP handler in your web framework of choice) - 1 to 3 steps, sub-30s run, no durable state, low volume, no human pause. Fast, cheap, tiny memory footprint.
- **Durable workflow engine** (e.g. a self-hostable workflow runner with a replay UI) - 4 to 20 steps, sub-hour, needs durable retry, replay UI, human pauses, or scheduled jobs that survive restart.
- **Heavy orchestrator** (e.g. a full distributed workflow cluster) - only if hundreds of workflows per second, multi-region polyglot workers, or 20+ steps with complex orchestration. Default answer: "You do not need this yet." Explain the bar.

### The matrix

| Requirement                       | Inline service | Durable workflow engine | Heavy orchestrator |
|-----------------------------------|----------------|-------------------------|--------------------|
| Steps per run                     | 1-3            | 4-20                    | 20+                |
| Max run duration                  | 30s            | 1h                      | hours+             |
| Survive container restart?        | no             | yes                     | yes                |
| Auto-retry transient failures?    | manual         | yes                     | yes                |
| Replay-a-single-step UI?          | no             | yes                     | yes                |
| Cron / scheduled?                 | system cron    | yes (in-task)           | yes                |
| Human-approval pauses?            | no             | yes (wait step)         | yes                |
| Multi-region / polyglot workers?  | no             | no                      | yes                |
| 1,000+ workflows/sec?             | no             | near limit              | yes                |
| Operational complexity            | low            | medium                  | high               |
| Memory footprint (steady-state)   | low            | medium                  | high               |
| Time to first deploy              | minutes        | about an hour           | days               |

**Rule:** if any row needs a "yes" you do not have in your chosen column, you have chosen wrong. Re-pick or accept the limitation.

### The lossy-restart risk (read this)

An inline service runs the entire workflow inside the request handler's call stack. If the container is killed mid-run (deploy, OOM, host reboot, dropped connection), the in-progress work is lost. No retry, no partial-completion replay, no "skip step 4, retry from step 5" UI.

This is the classic reason a multi-step handler outgrows inline. Consider a webhook handler that creates a record, then notifies a channel, then writes a follow-up. If it restarts after the record is created but before the notification, replay-on-restart can create a duplicate record. A durable engine checkpoints each step, so replay resumes cleanly.

**Heuristic:** if losing a single run silently in production would matter, do not use an inline service. The fix is not "add more logging", it is "use a durable engine".

The inline pattern is fine when:
- The whole thing fits in under 30 seconds.
- Each step is idempotent, OR the trigger source re-delivers on failure (most webhooks do).
- The work is mostly a single external call plus a database write.
- Volume is low enough that ad-hoc replay (eyeballing logs) is acceptable.

### Durable workflow engine - when it is the right answer

You want this when:
- You have 4+ steps and each costs real money or wall-clock time to repeat.
- A step might wait for a human ("approve this draft") or wait for an external event.
- You want a UI that shows every run, every step, every retry, and lets you replay from an arbitrary step.

What you accept:
- Higher worker memory than inline.
- An HTTP edge in front if you need HTTP triggers (workflow tasks are usually triggered via SDK, not addressable as HTTP by default).
- One round-trip latency to enqueue the task.
- Optionally a feature flag so you can fall back to inline if the engine is down.

### Heavy orchestrator - when (almost never)

Recommend only if:
- You need 1,000+ workflows per second sustained.
- You need polyglot workers (e.g. Go, Python, and TypeScript workers in the same workflow).
- You need multi-region active-active and cannot tolerate one node going down.

For most teams this is never the right answer. The cost is a full cluster, days of setup, and ongoing operational burden. If the user thinks they need it, push back: build it on a durable workflow engine first, revisit when you actually hit the documented ceiling.

## Step 3 - Show your reasoning

Print 4 to 8 lines:

```
Recommendation: <Inline service | Durable workflow engine | Heavy orchestrator>

Why:
- <2-3 bullets citing specific answers>
- <durability / volume / step count / pause requirement>

Trade-offs you accept:
- <what this choice does NOT give you>

Reply `yes` to scaffold, or override with `use durable` / `use inline` / `use orchestrator`.
```

The user must be able to override. Honor the override without arguing.

## Step 4 - Scaffold

Once confirmed, scaffold a minimal known-good boilerplate for the chosen runtime. Keep it minimal. Every new automation should get, by default:

- A `/health` endpoint returning `{ ok: true }`, wired into the container healthcheck.
- A top-level error path that notifies a channel on failure (do not let failures pass silently).
- A per-invocation run record (a log line or a row in a runs table) for visibility.
- Env validation with a schema (coerce empty strings to undefined, validate required keys at boot).
- A logger with redaction for secrets and common PII.
- Localhost-only port binding by default. Public exposure is a deliberate, separate step.
- A `.env.example` documenting every secret. Never commit real secrets.
- A Dockerfile and a compose file with sensible memory limits.

For an inline service, scaffold a single handler module plus the shared pieces above.
For a durable workflow engine, scaffold the task definitions plus a thin HTTP edge that triggers them, with each task wiring its own failure notification, plus the shared pieces above.
For a heavy orchestrator, do not scaffold. Print a short note on why this case might justify it and recommend building the durable-engine version first.

Use placeholders for the name, port, and any service-specific keys, then substitute them. Confirm the scaffold landed and no placeholder files remain.

## Step 5 - Print next-steps checklist

Template-specific. For an inline service:

```
Next steps for <name>:

  1. cp .env.example .env  ->  fill in secrets
  2. install dependencies
  3. run locally and hit http://localhost:<port>/health
  4. build the container and deploy it
  5. if HTTP-triggered, expose it (reverse proxy or tunnel) as a separate, deliberate step
  6. register it in your automation registry as status: dev
```

For a durable workflow engine:

```
Next steps for <name>:

  1. cp .env.example .env  ->  fill in secrets
  2. install dependencies
  3. provision an access token for the workflow engine and set it in .env
  4. set the project reference in the engine config
  5. deploy the tasks to the engine
  6. deploy the HTTP edge that triggers them
  7. flip the feature flag on (default off in the template)
  8. register it in your automation registry as status: dev
```

## Constraints

- Do NOT scaffold anything the user did not ask for. Templates are minimal.
- Do NOT add heavyweight frameworks, monorepos, or new top-level dependencies unasked.
- Do NOT push secrets. Templates contain only `.env.example`.
- Leave a clean tree unless the user asked you to commit.

## Edge cases

- **Pure cron, no steps, just hit a URL** - do not use any of these. Use a plain system cron that curls the endpoint.
- **Python-only work** - if a durable engine you are targeting is Node-only and the work is Python, use Python plus system cron, or a separate Python worker.
- **Mixed Node edge + Python workers** - use an inline edge that shells out to a Python script, or push to a queue and have Python consume.
- **Long-lived WebSocket or streaming** - none of these. Use a plain long-running service with a process manager or a dedicated container. Document it as not-an-automation.

## Reference - learn from how similar problems were solved

- **Inline-correct example:** a stateless proxy or transformer where the work IS the single external call. Losing one inflight request on restart is fine because the caller retries.
- **Inline-correct example:** a queue or pub/sub consumer that is idempotent by message ID. The queue itself is the durability layer, so a durable engine would be redundant.
- **Durable-correct example:** a multi-step handler that creates records and sends notifications across several external APIs, where a mid-run restart would otherwise create duplicates. Durable replay and per-step retry make the run UI the operator console.
- **Cautionary tale:** a hosted no-code workflow tool whose logic lives in opaque JSON exports. It makes code review impossible and breaks on upgrade. Prefer code-defined automations plus a run UI for the same operator experience with full source control.
