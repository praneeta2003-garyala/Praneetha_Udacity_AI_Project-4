# Harness Engineering Capstone — Final Submission

## Submission overview

This package contains the evidence and reflection for the four completed capstone systems. Evidence is organized by system under `evidence/system1` through `evidence/system4`.

The evidence is based on the student's actual local solution workspaces and recorded verification runs.

## Systems

### System 1 — Claims Intake Agent

Implementation:
`Build a Claims Intake Agent with a stop_reason-Driven Loop/exercises/03-dynamic-decomposition/solution`

Verification:
- 29 project tests passed.
- Successful full fixture run: `runs/20261007_184934`
- 8 claims processed.
- 7 routed to an adjuster.
- 1 escalated to a human.
- 0 incomplete claims.
- Trace artifacts show continuation on `tool_use` and termination on `end_turn`.

Evidence:
- `evidence/system1/task1_pytest_output.txt`
- `evidence/system1/runs/20261007_184934/`

### System 2 — Retail Support Copilot Context Strategy

Implementation:
`Engineer a Long-Conversation Context Strategy for a Retail Support Copilot/04-assemble-and-locate/solution`

Verification:
- 30 project tests passed.
- Successful run: `runs/20261008-162025`
- Baseline context: 38,708 tokens.
- Assembled context: 16,759 tokens.
- Reduction: 56.7%.
- Full evaluation: 6/6 passed.
- Control evaluation included a regression as required.

Evidence:
- `evidence/system2/task1_pytest_output.txt`
- `evidence/system2/runs/20261008-162025/`

### System 3 — Claude Code Multi-Surface Monorepo Team

Implementation:
`Configure Claude Code for a Multi-Surface Monorepo Team/04-plan-mode-and-explore-decision-doc/solution`

Verification:
- 35 project tests passed.
- `python -m ecommerce_team_config .` returned `OK`.
- Validator exit code: 0.

Key configuration evidence:
- `CLAUDE.md`
- `.claude/commands/review.md`
- `.claude/rules/`
- `.claude/skills/deploy-check/SKILL.md`
- `.claude/standards/`
- `docs/plan-mode-vs-direct-execution.md`

Evidence:
- `evidence/system3/task1_pytest_output.txt`
- `evidence/system3/validator_output.txt`
- `evidence/system3/CLAUDE.md`
- `evidence/system3/.claude/`
- `evidence/system3/docs/`

### System 4 — Multi-Shift Quality Monitoring

Implementation:
`Build a Multi-Shift Quality Monitoring System with Claude Orchestration/04-fork-scratchpad/solution`

Verification:
- 33 project tests passed.
- Shift C executed successfully using the recorded response.
- Warm database contains 13 Shift C records in the captured SQL-filtered slice.
- `data/hot_state.json` is valid JSON and 643 bytes.
- `data/shift_scratchpad.jsonl` was updated by the run.

Evidence:
- `evidence/system4/task1_pytest_output.txt`
- `evidence/system4/task2_shift_output.txt`
- `evidence/system4/task3_sql_slice_output.txt`
- `evidence/system4/hot_state.json`
- `evidence/system4/shift_scratchpad.jsonl`

## Verification summary

| System | Tests | Runtime evidence | Key result |
|---|---:|---|---|
| System 1 | 29 passed | Yes | 7 routed, 1 escalated, 0 incomplete |
| System 2 | 30 passed | Yes | 56.7% context reduction; 6/6 eval |
| System 3 | 35 passed | Validator | `OK`, exit 0 |
| System 4 | 33 passed | Yes | 643-byte hot state; 13-row SQL slice |

## Packaging notes

The final submission should exclude:
- `.venv/`
- `.pytest_cache/`
- `__pycache__/`
- generated Python package cache files
- local API credentials or secret environment values

The original solution `README.md` files may remain inside their respective solution/source folders. This top-level README is the submission index and evidence map.
