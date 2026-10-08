# Evidence Index

This folder contains the evidence artifacts used to verify the four capstone systems. The complete current solution/source for each system is also included under `solutions/system1` through `solutions/system4`.

## System 1

### `system1/task1_pytest_output.txt`
Captured project test execution showing the local System 1 test suite passing with 29 tests.

### `system1/runs/20261007_184934/`
Successful full fixture execution.

Key evidence:
- `summary.md` — aggregate claim outcomes.
- `traces/` — model/tool traces, including `stop_reason` behavior.
- `queues/` — routed work queues.
- `escalations.jsonl` — escalated claims.

Expected overall outcome from the verified run:
- 8 claims completed.
- 7 routed.
- 1 escalated.
- 0 incomplete.

## System 2

### `system2/task1_pytest_output.txt`
Captured project test execution showing 30 tests passed.

### `system2/runs/20261008-162025/`
Successful end-to-end context assembly and evaluation run.

Artifacts:
- `budget.json`
- `case_facts_call.json`
- `context.md`
- `eval.jsonl`
- `eval_control.jsonl`

Verified metrics:
- Baseline: 38,708 tokens.
- Assembled: 16,759 tokens.
- Reduction: 56.7%.
- Evaluation: 6/6 passed.
- Control: at least one regression observed.

## System 3

### `system3/task1_pytest_output.txt`
Captured project test execution showing 35 tests passed.

### `system3/validator_output.txt`
Captured validator execution for:

`python -m ecommerce_team_config .`

Expected result:
`OK`

Validator exit code: 0.

### Configuration evidence

- `system3/CLAUDE.md`
- `system3/.claude/commands/review.md`
- `system3/.claude/rules/api.md`
- `system3/.claude/rules/react.md`
- `system3/.claude/rules/tests.md`
- `system3/.claude/skills/deploy-check/SKILL.md`
- `system3/.claude/standards/`
- `system3/docs/plan-mode-vs-direct-execution.md`

These files demonstrate the project-level entry point, imported standards, path-scoped rules, review command, forked skill, and supporting standards.

## System 4

### `system4/task1_pytest_output.txt`
Captured project test execution showing 33 tests passed.

### `system4/task2_shift_output.txt`
Captured offline Shift C execution using the recorded response.

### `system4/task3_sql_slice_output.txt`
Captured SQL query against the warm database using:

```sql
SELECT id, ts, shift, component, severity, description
FROM defects
WHERE shift = 'C'
ORDER BY ts
```

The captured result contains 13 Shift C records.

### `system4/hot_state.json`
Generated hot state:
- valid JSON
- 643 bytes

### `system4/shift_scratchpad.jsonl`
Generated scratchpad entry for Shift C.

## Evidence integrity

Evidence in this package is intended to be copied from the student's actual solution workspaces after verification. No test output, run result, token count, or runtime artifact should be fabricated or manually rewritten to change its observed result.
