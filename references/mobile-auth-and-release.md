# Mobile authentication and release evidence

Use when native authentication, persisted sessions, shared web clients or binary compatibility affect the security invariant. Apply only the relevant flows; this is not a requirement to rebuild every app for every test.

## Authentication across real lifecycle boundaries

Check how each supported flow establishes intent, binds state/verifier/nonce and consumes the result. Distinguish PKCE links, provider callbacks, email codes, recovery links and explicit in-app sign-ins. Test unexpected raw tokens, wrong or ambiguous redirect paths, absent/expired intent, replay and concurrent delivery where relevant. Verify how an existing session should be preserved or intentionally replaced.

For PKCE, prove cryptographically secure randomness and the intended challenge method in the actual native runtime. Node or browser Web Crypto supplied by a harness cannot prove that an installed React Native binary has the required API or module. Check initialization order and failure behavior; do not accept an insecure fallback merely because a round trip succeeds.

On each in-scope OS, use the exact configured callback URI and record a warm launch and a terminated-process launch. Verify persisted intent/verifier state, duplicate event handling and cleanup after consumption. A recreated JavaScript client with mocked linking/storage proves only that simulated path. Record emulator/simulator versus physical device, binary revision, backend, platform version and which providers were real, substituted or not exercised.

## Confirmed account versus dispatched account

Inspect every caller of sensitive shared APIs, including web settings and legacy endpoint aliases. Model a session switch between confirmation, reauthentication and dispatch. A server correctly deriving identity from a bearer token can still act on the wrong account if a shared SDK silently attaches a newer session.

Bind the operation to the user identity and credential that were checked, or fail closed if they change. Confirm the actual outgoing authorization and authoritative target/survivor state. Include explicit-credential and default-SDK paths when both exist. Wrong-account proof, cancellation, retry and switching must not silently carry an old confirmation into a new account.

For logout/account switching, examine private caches, stored capabilities, subscriptions and in-flight work as well as the SDK session. Report source-only suspicions separately from reproduced leakage or mutation.

## Evidence needed for a release decision

Compare the candidate's native modules, permissions, entitlements, redirect schemes and build configuration with the currently shipped binaries. Added native requirements generally need fresh compatible binaries; a successful JavaScript export does not add a missing native module to an old installation. Verify the actual runtime compatibility mechanism and target build before recommending an over-the-air update.

Keep these claims separate:

- Unit/helper checks and real backend integration.
- Debug native build and emulator/simulator flow checks.
- Signed release candidate and physical-device/provider checks.
- Deployed configuration/source and store publication.

Record precisely which evidence exists; do not require all of it for every narrow finding or promote one category into another. Local runtime fingerprints and QA builds do not establish production parity. Real Apple/Google flows, provider revocation and store signing may remain explicit release gates.

Prepare a rollout order covering database, functions, web, mobile and older clients. Check that legitimate old-client behavior is either compatible or has an intentional upgrade path. Preserve rollback/recovery options. Obtain whatever release/production authority the user and project rules require; permission to test or prepare a build is not permission to publish it.
