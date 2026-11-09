# Harness Design

## Core loop

1. Receive a goal from the operator or upstream system
2. Planner decomposes it into an ordered step list with dependencies
3. Executor runs the next available step
4. If the step calls a tool, the tool router validates arguments against the registered schema and dispatches
5. Results return to the executor, which feeds them to the observer and memory layers
6. Replanner reviews progress and either advances, retries, or revises the plan
7. Loop until the goal is met or a termination condition fires

## Tool routing

- Each tool is registered with a name, JSON schema, and handler function
- Model output is parsed for tool-call blocks; arguments validated before execution
- Invalid arguments return a structured error to the model rather than crashing the loop
- Tool results are injected back into context in a canonical format

## Context management

- **Sliding window**: oldest non-critical turns dropped when the window fills
- **Summarization**: a cheap model call condenses dropped history into a compact summary injected as system context
- **Pinned content**: system prompt, tool schemas, and the current plan are never evicted

## Telemetry

Every step emits:

- Step ID and type
- Input tokens and output tokens
- Wall-clock latency
- Tool calls made and their results
- Planner decisions (advance / retry / replan)
- Errors with stack context

Aggregated into per-run and per-session dashboards.

## Multi-agent coordination

- Agents are registered with role descriptions and tool access
- Handoff is an explicit tool call: the current agent emits a structured transfer with context summary
- The receiving agent starts a fresh context seeded with that summary
- No shared mutable state between agents

## Failure modes handled

- Tool timeout: retry with backoff, then fail the step
- Model returns malformed tool call: structured error returned, model retries
- Infinite loop detection: step counter and plan-revision budget
- Context overflow: automatic summarization trigger
