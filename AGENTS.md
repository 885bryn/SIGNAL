# AGENTS.md — SIGNAL

## Authority

Repository documentation and Git history are authoritative. Read the current task before making changes.

Priority order:

1. `docs/PRODUCT.md`
2. `docs/ARCHITECTURE.md`
3. `docs/DECISIONS.md`
4. Approved `docs/features/F###-*.md`
5. Approved `docs/tasks/T###-*.md`
6. GitHub `main`
7. Chat history only as supporting context

If these disagree, stop and report the conflict. Do not silently rewrite architecture to match accidental implementation.

## Architecture guard

Preserve the approved architecture. Stop and report instead of improvising if work would:

- move final classification, history, baselines, anomaly scoring, GPS ownership, or primary UI away from Android;
- duplicate an RF domain that already has an authoritative sensor without an approved fallback/testing reason;
- add active interference, packet injection, deauthentication, jamming, BLE spam, MouseJack/injection, or similar transmit/attack behavior;
- claim measured direction, exact range, or exact transmitter location from RSSI/observer-location data;
- bypass approved module or licensing boundaries;
- introduce a major dependency/framework or change an important interface/data model unexpectedly;
- weaken tests merely to make work pass;
- substantially expand scope beyond the approved task.

Report architecture conflicts as `ARCHITECTURE DRIFT DETECTED` and return the decision to Sol.

## Engineering policy

- Reuse before implementation.
- If upstream already satisfies a requirement, wrap/adapt it rather than rewrite it.
- Modify upstream code minimally until end-to-end functionality exists.
- Keep radio acquisition passive.
- Hardware emits observations/evidence; Android owns final human-facing classification.
- Preserve evidence and confidence for inferred labels.
- Surface sensor failures explicitly; never silently switch to a fallback radio.
- Maintain upstream commit provenance, license notices, and local-modification records.
- Avoid unrelated refactors.
- Prefer existing repository patterns.

## Codex workflow

For each implementation task:

1. Inspect Git state and sync `main` safely.
2. Create or switch to the task branch specified by the task file.
3. Read `AGENTS.md`, the task, referenced feature, architecture, and decisions.
4. Inspect only the necessary code first.
5. Implement only approved scope.
6. Use tests/TDD where practical and systematic debugging for failures.
7. Run the required build/tests.
8. Inspect the final diff for unrelated changes and architecture drift.
9. Commit only relevant files.
10. Push the task branch.
11. Return the branch, commit hash, files changed, test/build results, unresolved issues, and architecture concerns.

Use the lowest-capable Codex model for mechanical work and Luna for normal well-specified implementation. Escalate only when the remaining difficulty is genuinely implementation-specific.
