@AGENTS.md

Validation scope, shared capacity and remote execution follow AGENTS.md. Command examples below do not require a full-suite rerun for every edit.

## Claude Code

- Verify after edits: run the `-only-testing:StudentWatchTests` command from AGENTS.md; run the full `test` command when touching layout or UI copy. If `xcrun simctl list runtimes` is empty, fall back to the `build` command and report that tests were not run.
- `ClassOneViewController.swift` is ~3.4k lines: use Grep with the seat name (e.g. `tableSeven2`) and read ranges instead of reading it whole.
- A change that touches all 32 seats is a mechanical multi-block edit; use plan mode and confirm the pattern on one seat first.
