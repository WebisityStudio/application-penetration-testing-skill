# Supabase runtime testing

Use for dynamic tests of Supabase-backed applications. Choose available tools from their installed help and verified documentation; this reference makes no claim about an official Codex Security CLI or its commands. A repository scanner can supply hypotheses, but runtime validation still requires scoped interfaces and an observable test environment.

## Runtime and identity separation

Prove local reachability before starting a long run. Use a disposable hosted project only when authorized; inability to reach localhost does not grant permission to create infrastructure or test production.

Reproduce relevant migrations in effective order, grants, RLS, security-definer RPCs, Realtime policies, Storage policies, Edge source and Auth configuration. Record server/runtime versions and differences from the intended deployment. Confirm the worker loads the intended files. Replace integrations with sinks and record which flows cannot be represented faithfully.

The attacker can receive the test URL, public anon/publishable key, synthetic credentials or short-lived ordinary sessions, and issued share/invitation tokens. The controller retains service-role/secret keys, database-owner and management credentials, deployment access and teardown authority. A public key does not itself supply a user identity or make a project authorized for testing.

Tag fixtures with a run identifier where possible and retain an inventory otherwise. The controller creates users/tenants, changes membership, reads protected effects and removes owned fixtures. Test requests use real Auth sessions through REST, RPC, Storage, Edge or websocket interfaces. Never substitute controller credentials to make a failing attacker request work.

## Boundary tests

| Boundary | Useful attack and control |
| --- | --- |
| RLS / Storage | Other-user and cross-tenant reads/writes versus valid own-object operations; inspect authoritative state and metadata leakage |
| RPC / Edge | Forged user, owner, location or event fields versus a valid request; check server-derived identity and privileged function behavior |
| Bearer capabilities | Expiry, revocation, reuse and object rebinding versus an active authorized token |
| Realtime | Establish delivery before removal; then test the existing connection and a new subscription separately, with an authorized subscriber still receiving events |
| Abuse bounds | Concurrent and bulk requests, retry/conflict paths, size and total-duration limits versus ordinary traffic; verify quotas and effects, not just status codes |
| Session lifecycle | Logout, expiry, revocation and account switching across cached state and all shared clients |

For revocation, define the product's required propagation bound. Preserve connection state, event sequence, removal readback and observation interval. Silence only means rejection when the source and an allowed receiver remain demonstrably live. A refused new subscription does not prove an already-open one stops leaking.

For streaming bounds, identify whether the result uses the public gateway, a real worker helper endpoint or a mocked stream. Measure consumed bytes, cancellation and the complete response deadline with a valid-payload control. A gateway that supplies missing headers may prevent reproducing a helper flaw through the original network path; report that distinction.

## Recent authentication: test where the proof came from

Start with the intended attacker capabilities, such as an old stolen session without mailbox, provider or existing-factor access. Test whether that session can change or add a password, email, phone, linked identity or factor and then use it to manufacture accepted recent-auth proof. A new timestamp alone is not evidence of a new independent factor.

Cover existing-password and passwordless accounts separately. Requiring the previous password may protect replacement but leave first-password creation exposed. Check the actual provider/settings/version and allowed credential-management paths before claiming a proof source is independent. A newly enrolled factor controlled by the attacker is not automatically stronger proof.

For destructive actions, test expired, malformed, wrong-account, wrong-purpose and reused proof as applicable, plus a legitimate completion. Derive identity from verified server credentials and bind the client's confirmed account to the dispatched request; see the mobile/shared-client reference. Test every supported route or alias that enforces the same policy.

Do not turn one app's proof window, enabled providers or Auth settings into universal requirements. Record required assumptions, compare them with read-only deployed observations when authorized, and keep production configuration changes separate from test preparation.

## Effects and closure

Route location/SOS/check-in notifications to sinks with synthetic coordinates and no real device push tokens. Use disposable accounts for deletion and hard request ceilings for quota/load probes. Verify survivors as well as the intended mutation.

Snapshot any temporarily changed grants/configuration. Restore them and verify fixture counts/state before the next suite. Separate the attack verdict from cleanup failures and production-parity limitations. Preserve the exact exploit and positive controls as regressions where practical.
