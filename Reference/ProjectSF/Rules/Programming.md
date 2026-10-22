# Programming Rule — curated public summary

> **Curated summary, not verbatim.** Based on private ProjectSF `AI/Rules/Programming.md`, source `main` `08715c68b04f` (2026-10-22). Original file includes project-local dependencies; this excerpt is **not** a separately executable policy.

## Standard First, Simple is Best

Use the current engine/platform's **official feature and documentation**, established industry practice and existing project responsibilities before adding a helper, manager, service, plugin or framework. Exceptions require a concrete mismatch, trade-off, scope and verifiable benefit. **Simple** means lower overall understanding, modification, verification and operational cost—not merely fewer lines or fewer files.

## Inspect before modifying

- Check current repository status, related diffs, consumers, lifetime, error handling, tests and data ownership.
- Reuse an existing feature, then extend it, and add a new component only if responsibility boundaries justify it.
- Keep protocol generation, serialized data and producer/consumer contracts coherent; do not manually edit generated outputs.
- Preserve user edits and avoid unrelated refactoring or speculative fixes.
- Address the observed cause, not just the error message; verify meaningful normal, boundary and failure behaviors.

## Completion boundaries

Build success, test success, actual product behavior, Creator acceptance and publication are different claims. Do not infer `PASS` from implementation alone. Document-only changes get documentation checks rather than mandatory Unreal builds. Publish/commit only within the requested scope.

See [Verification Depth — verbatim source](VerificationDepth.md) and [the inspection boundary](../../../Docs/SourceAudit-2026-10-22.md).
