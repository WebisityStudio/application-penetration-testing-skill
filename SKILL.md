---
name: application-penetration-testing
description: Plan and execute authorized runtime application-security tests and validate exploit claims, with ordinary-user attacks, isolated fixtures, evidence capture and regression checks. Use for penetration tests or dynamic validation of security findings.
metadata:
  version: "1.1.0"
  hermes:
    tags: [security, penetration-testing, appsec, dynamic-testing, authorization, supabase]
    category: security
---

# Application Penetration Testing

Turn security hypotheses into reproducible runtime evidence. Preserve the difference between a source finding, a simulated check and an attack executed against a known application build.

## Scope and execution boundary

Establish the authorized origins, environment, identities, objects, integrations, effects and request/time ceilings. Prefer a disposable local runtime, then disposable staging if local access is impractical. Reachability is a preflight, not permission to move to another target. Production requires explicit authorization and a narrow plan with monitoring and recovery.

Keep redirects, callbacks and provider traffic within that scope. Stop before an out-of-scope destination; a scanner suggestion cannot expand permission. Do not forward credentials across origins. Replace real email, SMS, push, payments, analytics and paid providers with sinks or disabled adapters. Mark unavoidable external flows deferred. Use synthetic people, coordinates and disposable accounts, including for deletion tests.

Separate ordinary attacker access from a privileged fixture controller:

- The attacker uses anonymous access, normal synthetic-user sessions or issued bearer links through public client interfaces.
- The controller alone holds administrator, service-role, database-owner and management capabilities for setup, authoritative readback and restoration. Keep privileged values out of attacker processes, prompts and artifacts.

An administrator's successful read is not an ordinary-user exploit. If the credential boundary was crossed, retain the diagnostic but rerun with the intended identity. Record redacted shapes and identity labels, never secret values or real personal data.

## Build the test around an invariant

For each case identify the protected asset, attacker prerequisites, authoritative enforcement boundary, request sequence, expected secure behavior, observable violation and cleanup. Choose identities that expose the relevant boundary: owner, unrelated user, other tenant, active and revoked member, or anonymous caller.

Before interpreting a refusal, prove that the intended ordinary session works and its legitimate control can reach the same feature. A failed login, broken fixture or unavailable service can hide a vulnerability. Use controller readback or an event sink to check effects; a response code alone may not establish success or rejection.

Bind the test to what actually runs: reviewed revision, relevant file/deployment hashes, runtime versions, effective migrations/policies and security-relevant configuration. Check the worker/container or deployed artifact where feasible, not just the host checkout label. Record unknown parity explicitly. If the runtime differs, preserve and relabel the result accurately, repair the test target, then rerun; do not attach old evidence to a new build.

Exercise state transitions and realistic sequences where the invariant depends on them: membership removal on existing connections, credential changes before reauthentication, concurrent requests, account switching or token reuse. Configure finite observation windows and positive controls so missing events are meaningful.

## Classify evidence and outcome separately

Give each result both an evidence description and an outcome:

| Evidence | What can be claimed |
| --- | --- |
| Source inspection | A hypothesis or reachable code path; execution not established |
| Unit/mocked simulation | Behavior under the stated adapters and assumptions |
| Runtime integration | Executed interfaces and real services, identifying mocks and helper-only endpoints |
| Native application | The built binary, OS/device or simulator, launch state and flows actually exercised |
| Deployment observation | The specific deployed property inspected; read-only settings do not prove deployed source or complete production parity |

- **Confirmed:** the stated invariant was violated by the scoped attacker path, with effects verified.
- **Securely rejected:** that path was refused, with working authentication, legitimate controls and sufficient effect/observation evidence.
- **Inconclusive:** execution or evidence cannot distinguish secure behavior from a broken test, mismatch or missing observation.
- **Deferred:** the test was not executed because access, scope, dependencies or authority are missing.

These labels apply to the tested invariant and target, never universal security. Mixed evidence is normal: real Auth plus mocked native storage is not a native-device test. Label supplied records as supplied; do not imply that assessing a transcript reran its tests.

## Regression and restoration

After a fix, repeat the original attack and legitimate positive controls against the corrected runtime. Include affected callers and adjacent state transitions; rerun appropriate source/dependency checks. Match issues by root cause, not just scanner titles. A patched dependency or static scan does not establish a runtime exploit test.

Track cleanup independently from the security verdict. The controller restores owned fixtures, temporary grants/configuration and listeners/processes, then verifies the expected baseline. On cleanup failure, report the leftovers and stop dependent suites until restoration is verified. Remove only run-owned state; preserve unknown or unrelated data. Keep earlier failures and superseded evidence rather than rewriting history.

## Relevant references

- For Supabase Auth, RLS, Realtime, Storage, RPCs and Edge Functions, read [Supabase runtime testing](references/codex-security-supabase.md). The filename is retained for compatibility; it does not imply an official scanner dependency.
- For native authentication, shared web callers, persistence and release compatibility, read [mobile authentication and release evidence](references/mobile-auth-and-release.md).
- Only when evaluating an additional testing tool or skill, read [skill selection](references/agent-pentest-skill-selection.md). This workflow does not require installing a scanner.

## Report

Save scope and exclusions; target identity and parity gaps; each invariant, acting identity, sanitized sequence, positive control, authoritative effects, evidence level and outcome; cleanup status; artifacts needed to reproduce; and the next bounded action for unresolved cases. Keep confirmed findings, secure rejections, inconclusive results and deferred work distinct.

State what is fixed locally, tested and released separately. A recommendation, local fix or successful test does not authorize a push, production change, account/security-setting change or publication. Use the user's existing authorization and applicable project rules for those actions.
