# Harness Engineering Capstone — Reflection Brief

## 1. Overall approach

Across the four systems, I treated the harness as part of the engineering solution rather than as a thin wrapper around implementation code. The work separated three concerns: model-facing behavior, deterministic harness enforcement, and orchestration/state management. The strongest results came from making those boundaries explicit and then verifying them with repeatable tests and captured runtime artifacts.

## 2. System 1 — Claims Intake Agent

The Claims Intake Agent uses a stop-reason-driven loop. The important design decision is that loop continuation is controlled by the model/API `stop_reason`, not by parsing assistant prose or relying on a fixed iteration script.

The loop behavior is:
- continue when `stop_reason == "tool_use"` and execute the requested tools;
- terminate when `stop_reason == "end_turn"`;
- treat unexpected stop reasons as errors rather than silently continuing.

The implementation also uses terminal-tool state so that a claim is not considered complete until a routing or escalation action has actually occurred. This gives the harness a deterministic completion condition even when the model's natural-language response is incomplete.

The verified full run was `runs/20261007_184934`. It completed all eight fixtures with 7 routed claims, 1 escalated claim, and no incomplete claims.

The anti-pattern avoided here is using a brittle fixed-iteration conversational loop or attempting to infer tool intent from assistant text. Those approaches make completion dependent on wording and can leave records unfinished.

## 3. System 2 — Retail Support Copilot

The context strategy separates durable case facts and resolved information from the large active conversational context. The objective is to reduce prompt size while preserving the information needed to answer the evaluation questions.

The verified run was `runs/20261008-162025`.

Observed measurements:
- baseline context: 38,708 tokens;
- assembled context: 16,759 tokens;
- reduction: 56.7%.

The full evaluation passed 6/6 questions, while the control evaluation retained a regression. This demonstrates that the reduction did not simply discard everything; it preserved the information needed for the tested answers while still changing the context representation.

The key engineering lesson is that long-context optimization should preserve information by semantic role and retrieval need, rather than treating every previous message as equally important.

## 4. System 3 — Claude Code Multi-Surface Monorepo Team

System 3 treats Claude Code configuration as a layered policy surface.

`CLAUDE.md` is the project-level entry point and imports the shared standards. Path-scoped rules add guidance only where the matching source paths require it. The `/review` command defines a repeatable, read-only review workflow, while the `deploy-check` skill uses a forked execution context and read-only tools.

This layering keeps broadly applicable standards separate from path-specific guidance and separates reusable workflows from the main conversational context.

The implementation was verified with 35 passing tests. The validator returned `OK` with exit code 0.

## 5. System 4 — Multi-Shift Quality Monitoring

System 4 focuses on durable state, crash recovery, and isolation of exploratory reasoning.

The system separates:
- hot state for a compact recent state representation;
- warm SQLite state for queryable operational history;
- scratchpad state for append-only working notes;
- forked scratchpads for isolated hypotheses.

The verified Shift C run used the recorded response and generated a 643-byte `hot_state.json`. A separate SQL query captured a 13-row Shift C slice from the warm database. The scratchpad was also updated by the run.

The crash-recovery tests establish that incomplete writes do not erase previously complete scratchpad lines and that recovery decisions depend on the recorded state and age threshold. Fork tests establish that exploratory branches do not mutate the base scratchpad and that separate forks remain independent.

The main recovery lesson is to make recovery deterministic from durable state, while keeping speculative or exploratory work isolated from the authoritative state.

## 6. Model layer, harness layer, and orchestration layer

### Model layer

The model layer is responsible for reasoning and selecting actions from the available tools or context. System 1 demonstrates this through tool-use decisions, while System 2 demonstrates it through answering questions from an assembled context.

### Harness layer

The harness layer defines the deterministic contract around the model. System 1 uses stop reasons and terminal actions to define completion. System 3 uses configuration files, path-scoped rules, and validator checks to define how the coding assistant should operate.

### Orchestration layer

The orchestration layer manages state and multi-step execution. System 4 is the clearest example: state is tiered, recovery is explicit, and forked scratchpads provide isolation for hypotheses.

The separation matters because failures at one layer should not be hidden by another. Model behavior can vary, but the harness should still enforce completion and validation, while orchestration should preserve recoverable state.

## 7. Deterministic controls versus prompt guidance

A recurring lesson across the project is that important invariants should be enforced in code or configuration rather than only described in prompts.

Prompt guidance is useful for explaining intent, workflow, and preferred reasoning behavior. Deterministic controls are better for conditions such as:
- whether a claim is actually routed or escalated;
- whether a validator returns success;
- whether hot state exceeds its budget;
- whether a scratchpad fork can mutate its base.

This distinction makes the system more testable and reduces the chance that a model interpretation silently violates a system invariant.

## 8. System 2 versus System 4 context strategy

System 2 and System 4 solve different context problems.

System 2 addresses a conversational context window directly. It assembles a smaller context from durable and active sections so that the model receives less irrelevant history while retaining answer-critical information.

System 4 addresses operational state over a longer-running workflow. Rather than repeatedly sending all historical records, it uses tiered state: compact hot state, queryable warm storage, and append-only scratchpad state, with recovery and fork isolation.

System 2 therefore optimizes what the model sees in a conversation, while System 4 optimizes what the system keeps and retrieves across operational time.

## 9. Final reflection

The main outcome of the capstone is a clearer distinction between model capability and engineered reliability. A capable model can select useful actions, but reliable systems need explicit completion conditions, bounded context, validated configuration, durable state, and recoverable orchestration.

The four systems together show that harness engineering is not only about adding checks around an agent. It is about designing the boundaries so that model behavior remains useful while critical system properties are deterministic, inspectable, and testable.
