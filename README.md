# Claude Harness Engineering Portfolio

A portfolio case study in **agentic harness design, context engineering,
Claude Code configuration, and long-running orchestration**.

> This repository is a public-facing portfolio summary. It intentionally
> excludes course starter code, API keys, private transcripts, and raw
> course-only materials.

## What I Built

I developed and evaluated four complementary AI-engineering systems:

1.  **Stop-reason-driven agentic loop** --- a tool-using harness whose
    control flow follows model protocol state (`tool_use`
    vs. `end_turn`) rather than a hard-coded turn count.
2.  **Context engineering pipeline** --- a context assembly strategy
    that reduced a 38,708-token baseline to 16,935 tokens while
    retaining 6/6 evaluation answerability.
3.  **Claude Code configuration harness** --- modular project guidance
    using `CLAUDE.md`, imported standards, path-scoped rules, project
    commands, and forked read-only skills.
4.  **Tiered orchestration/state system** --- SQL-filtered warm state,
    bounded hot state, durable scratchpads, crash recovery, and isolated
    hypothesis forks.

## Measured Results

  -----------------------------------------------------------------------
  Area                                                             Result
  ------------------------------ ----------------------------------------
  Context baseline                                          38,708 tokens

  Assembled context                                         16,935 tokens

  Context reduction                                            **56.25%**

  Context evaluation                                              **6/6**

  Warm-tier defect history                                     40 records

  New defects analyzed in                                           **2**
  example shift                  

  Hot-state artifact                                        **680 bytes**

  System 1 deterministic tests                                  29 passed

  System 2 deterministic tests     30 passed in the submitted environment

  System 3 deterministic tests                                  35 passed

  System 4 deterministic tests                                  33 passed
  -----------------------------------------------------------------------

## Architecture

``` mermaid
flowchart LR
    M[Model] --> H[Harness]
    H --> T[Deterministic Tools]
    H --> C[Context Assembly]
    H --> O[Orchestration]
    O --> HOT[Hot State]
    O --> SQL[(Warm SQLite)]
    O --> SP[Durable Scratchpad]
    O --> F[Isolated Forks]
```

The design separates **model judgment** from **deterministic
enforcement** and from **longer-lived orchestration state**.

## Engineering Highlights

### Agentic control

The loop continues when the model returns `tool_use`, executes requested
tools, appends tool results, and calls the model again. It returns on
`end_turn`, while unsupported stop reasons are treated as explicit
protocol errors.

### Context engineering

The context strategy reduced the assembled prompt by **56.25% (38,708 →
16,935 tokens)**. High-value active information remained verbatim while
resolved material was compressed. The resulting context answered **6/6
evaluation questions**.

### Claude Code configuration

The configuration used modular project standards, path-scoped test rules
such as `**/*.test.tsx` and `**/*.test.ts`, project-scoped commands, and
forked read-only skills to reduce context pollution and limit blast
radius.

### Long-running orchestration

The orchestration layer pushed historical filtering into SQL instead of
loading the full history into model context. In the documented run, the
warm tier held **40 defects**, while only **2 new defects** were
analyzed. The hot state remained only **680 bytes**.

Forked investigations copied baseline state into isolated working areas.
Findings could then be merged intentionally into the main scratchpad
without rewriting the base state during investigation.

## Production Lesson

A real model run exposed an important distinction between deterministic
correctness and model behavior: some claims ended before reaching a
terminal business route even though the harness test suite passed.

A production extension would add a **lifecycle guardrail**:

``` text
model end_turn
     |
     v
terminal route/escalation reached?
     | yes                 | no
     v                     v
   return        bounded recovery / escalation
```

This preserves model discretion while enforcing the business invariant
that every claim must reach a terminal lifecycle state.

## Repository Guide

-   `agentic-loop/` --- protocol-driven tool loop design
-   `context-engineering/` --- token-budget and context-layout case
    study
-   `claude-code-harness/` --- configuration architecture
-   `orchestration/` --- state tiers, recovery, and fork isolation
-   `docs/architecture.md` --- architecture explanation
-   `docs/results.md` --- measured results and interpretation
-   `evidence/results.json` --- sanitized numerical evidence

## Skills Demonstrated

Python · Agentic systems · Tool use · Context engineering · Prompt/token
budgeting · Claude Code · Configuration-as-code · SQLite · State
management · Recovery design · Testing · AI reliability engineering

## Publication Note

This portfolio intentionally presents original analysis, architecture,
measured outcomes, and sanitized evidence rather than republishing a
complete course repository. Before publishing any course-derived source
code, I would verify the current Udacity sharing/honor-code terms and
remove secrets or sensitive artifacts.
