# Execution Observability — curated public summary

> **Curated summary, not verbatim.** Inspected private ProjectSF `AI/Workflows/ExecutionObservability.md` at `08715c68b04f` (2026-10-22).

Progress reporting is a way for the **Creator** to understand what has actually happened and what will happen next, not a compulsory report schema, extra CLI, pause point or new permission boundary.

## Reporting

- Start/resume identifies the selected **Issue / Spec / direct request** and actual known state.
- Report meaningful results and the next approved action; continue within the authorization boundary without requiring a user to type “continue” after each progress report.
- Distinguish candidate versus **applied** Skill, discovered versus **actually invoked** Tool, and delegated results versus Main's integrated verification.
- Distinguish tool/build/save success, technical/product validation, Creator acceptance and remote-main publication.
- If not observed, say `NOT_TESTED` / unknown instead of inventing a status or speedup.
- Keep secrets, raw logs and hidden reasoning out of traces.

A detailed execution trace **may** expose Request/Step, applied Skill, actual Tool, artifact, observed validation and next action. Small reads/edits need no template.

## Continuation and completion

Before resuming, reconcile the latest original Issue or Spec state with the repository/Editor/Evidence; do not trust previous conversation state. A final task summary reports the changed artifact and verified scope, applicable Skill and tools when relevant, outstanding decisions/limitations, and whether publication occurred. A progress update by itself is **neither final completion nor a new approval request**.

See [TaskExecution source](TaskExecution.md) and [inspection scope](../../Docs/SourceAudit-2026-10-22.md).
