# Telemetry Specification

## Event schema

Every harness event is a JSON object with these required fields:

```json
{
  "event_id": "uuid",
  "timestamp": "ISO-8601",
  "session_id": "string",
  "run_id": "string",
  "step_id": "string",
  "event_type": "plan | execute | tool_call | tool_result | observe | replan | error | terminate",
  "payload": { }
}
```

## Event types

### plan
Emitted when the planner produces or revises a step list.
Payload: ordered steps, dependencies, estimated complexity.

### execute
Emitted when a step begins and ends.
Payload: step description, start/end timestamps, outcome.

### tool_call
Emitted when a tool is invoked.
Payload: tool name, validated arguments, call ID.

### tool_result
Emitted when a tool returns.
Payload: call ID, result or error, latency.

### observe
Emitted by the observer layer.
Payload: aggregated metrics for the step.

### replan
Emitted when the planner revises strategy.
Payload: reason, old plan, new plan.

### error
Emitted on any failure.
Payload: error class, message, stack trace, recovery action taken.

### terminate
Emitted when a run ends.
Payload: final status, total steps, total tokens, duration.

## Aggregation

- Per-run: token totals, step counts, error counts, duration
- Per-session: run history, cumulative cost, recurring failure patterns
- Cross-session: tool reliability rates, planner revision frequency

## Export

Events stream to stdout as JSONL by default. Optional sinks: file, HTTP endpoint, or message queue.
