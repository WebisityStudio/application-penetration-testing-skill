# Application Penetration Testing Skill

An agent skill for authorized application-security testing: turn suspected
vulnerabilities into reproducible evidence, verify fixes, and explain what a
test actually proves.

The skill is a set of instructions and references. It does not install a
scanner or include an autonomous exploit engine. Your agent uses the tools
available in your environment within the scope you authorize.

## What it covers

- Ordinary-user attacks with privileged fixture setup and verification kept separate.
- Working positive controls, runtime/build identity and authoritative effect checks.
- Supabase Auth, RLS, RPC, Storage, Realtime and Edge Function boundaries.
- Authentication-proof origin, passwordless accounts and account-switch races.
- Native PKCE, callback lifecycles and mobile binary/update compatibility.
- Bounded side effects, cleanup verification and regression testing.
- Distinct outcomes: confirmed, securely rejected, inconclusive and deferred.

Start with [SKILL.md](SKILL.md). It links the platform references only when they
are relevant to the task.

## Use it

Clone the repository into a local working directory:

```sh
git clone https://github.com/WebisityStudio/application-penetration-testing-skill.git
```

Give your coding agent the entrypoint and a concrete authorized scope:

> Read `application-penetration-testing-skill/SKILL.md` and use it to assess my
> local application. Only `http://127.0.0.1:58000` and disposable synthetic
> accounts are in scope. Keep real email, push and paid providers disabled.
> First establish the test environment and invariants, then run bounded tests,
> verify legitimate controls and cleanup, and report evidence and gaps.

Replace the example origin and constraints with your actual authorized target.
Source access or a reachable URL alone does not authorize runtime attacks.

### Codex installation

To make it discoverable as `application-penetration-testing`, clone it into your
configured personal skills directory:

```sh
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
git clone https://github.com/WebisityStudio/application-penetration-testing-skill.git \
  "${CODEX_HOME:-$HOME/.codex}/skills/application-penetration-testing"
```

If that destination already exists, inspect your existing installation and
preserve local changes before updating it. Do not overwrite it blindly.
Start a new agent session and request the skill by name.

For other agents that support `SKILL.md`, use their documented skills directory
or provide the entrypoint directly. The package includes Hermes metadata;
format/parser compatibility was checked locally. Compatibility with every
agent or platform has not been established.

## Repository contents

| Path | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Shared workflow, boundaries and reporting contract |
| [references/codex-security-supabase.md](references/codex-security-supabase.md) | Supabase runtime and identity testing; historical filename retained |
| [references/mobile-auth-and-release.md](references/mobile-auth-and-release.md) | Native auth, shared callers and release evidence |
| [references/agent-pentest-skill-selection.md](references/agent-pentest-skill-selection.md) | Evaluate additional testing tools only when needed |
| [validation/](validation/README.md) | Constructed scenarios, rubric, assessed answers and results |

## Validation and limits

Version **1.1.0** passed ten constructed tabletop scenarios in an independent
Codex-agent grading pass. The cases check reasoning about authentication,
authorization, provenance, mobile evidence, scope, cleanup and release
authority. The evaluated skill files are identified by hashes in
[validation/results.json](validation/results.json).

These results are not a live penetration test, a security certification or a
general success rate. There was one answer per scenario, no held-out test set
and no zero-skill control. Full limitations and a repeatable evaluation method
are in the [validation guide](validation/README.md).

## Provenance

Maintained by [WebisityStudio](https://github.com/WebisityStudio). This package
was adapted from a locally maintained skill and revised with Codex assistance.
The earlier skill's upstream authorship has not been established. This is not
an official OpenAI or Anthropic skill, and no vendor endorsement or independent
Opus review is claimed.

## Contributing

Keep the shared entrypoint concise and place platform-specific detail in the
references. For a behavioral change, provide a synthetic scenario that exposes
the decision at issue and evaluate it without giving the answering agent the
rubric. Preserve positive controls, evidence limits and restoration checks.
Do not include real credentials, customer data or private application evidence
in contributions.

## License

Released under the [MIT License](LICENSE).

Copyright (c) 2026 WebisityStudio.
