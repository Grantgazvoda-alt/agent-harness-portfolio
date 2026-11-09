# Agent Harness

**LLM agent orchestration framework.**

A harness for building, running, and observing multi-step AI agents — tool routing, context management, planning loops, and execution telemetry.

## What it does

- **Tool routing**: maps model-emitted tool calls to registered handlers with schema validation
- **Context management**: sliding-window and summarization strategies for long-running sessions
- **Planning loops**: decompose goals into steps, execute, observe results, replan
- **Execution telemetry**: structured logs of every step, token, and decision
- **Multi-agent coordination**: handoff protocols between specialized agent roles

## Why it matters

Raw model APIs are not agents. An agent needs a loop, a tool layer, memory, and observability. Most teams build this ad hoc and hit the same walls: context overflow, silent tool failures, and no visibility into why the agent did what it did.

This harness treats those as solved infrastructure problems, not per-project puzzles.

## Architecture

```
┌──────────────┐
│ Goal / Task  │
└──────┬───────┘
       ▼
┌──────────────┐
│ Planner      │  decompose into steps, track dependencies
└──────┬───────┘
       ▼
┌──────────────┐
│ Executor     │  run steps, invoke tools, capture results
└──────┬───────┘
       ▼
┌──────────────┐
│ Observer     │  log decisions, tokens, latency, errors
└──────┬───────┘
       ▼
┌──────────────┐
│ Memory       │  context window management, summarization
└──────┬───────┘
       ▼
┌──────────────┐
│ Replanner    │  adjust plan from observed results
└──────────────┘
```

## Design principles

- **Observable by default**: every step emits structured telemetry — no black boxes
- **Fail loud**: tool errors surface immediately with full context, not swallowed
- **Stateless core**: the harness holds no session state; memory is an explicit layer
- **Model-agnostic**: the planner and executor work with any function-calling model

## Status

Active development. Architecture and design docs public; implementation retained as proprietary IP.

## Contact

Open to contract and employment discussions in AI engineering, agent infrastructure, and LLM application development.
