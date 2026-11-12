<p align="center">
  <img src="https://github.com/LinuxHello/.github/raw/main/profile/linuxhello-logo.svg" alt="Linux Hello logo" width="240">
</p>

# Linux Hello

[![Quality gate status](https://sonarcloud.io/api/project_badges/measure?project=LinuxHello_linux-hello&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=LinuxHello_linux-hello)
[![AI Code Assurance](https://sonarcloud.io/api/project_badges/ai_code_assurance?project=LinuxHello_linux-hello)](https://sonarcloud.io/summary/new_code?id=LinuxHello_linux-hello)

Passkeys and secrets for the Linux desktop, rooted in a PIN and unlocked by biometrics: a session credential broker built on a virtual CTAP authenticator, fprintd, Howdy, KeePassXC and TPM 2.0 ([ADR-0005](https://github.com/LinuxHello/architecture/blob/main/adr/0005-provider-path.md)). The homepage is [linuxhello.github.io](https://linuxhello.github.io/).

**Status: pre-alpha.** Nothing here authenticates anyone yet. There are no releases. The Agent's state API, its confinement and the policy layers exist; a TPM-backed virtual authenticator works as a prototype with libfido2, Chromium and Firefox ([spike 0094](https://github.com/LinuxHello/architecture/blob/main/spikes/0094-virtual-ctap-end-to-end.md)). The [README of `linux-hello`](https://github.com/LinuxHello/linux-hello#readme) says what is built.

## What it does

- **Unlock once.** Face, fingerprint or PIN unlocks the session; passkeys then work in browsers, native apps and Electron apps through a virtual security key (a `uhid` device), without patching the browsers.
- **The PIN is the root.** Keys hang off a per-user TPM root key that the PIN authorises. Biometrics only satisfy a TPM policy. The account password authorises a destructive reset, never an unlock.
- **Per relying party custody.** A routing policy decides for each relying party whether its key is device-bound (TPM) or portable (KeePassXC).
- **Keys sign, never leave.** Private keys stay in the TPM or KeePassXC.
- **KDE Plasma first.** One prompt that shows which method is active, the same on the login manager, the lock screen and in the session.

## Repositories

| Repository                                                 | Content                                                                                                                |
| :--------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------- |
| [linux-hello](https://github.com/LinuxHello/linux-hello)   | The code (one crate per container), the security properties and the policy semantics                                   |
| [architecture](https://github.com/LinuxHello/architecture) | The architecture (Arc42 sections, C4 model), the decision records and the spikes; `linux-hello` pins it as a submodule |
| [branding](https://github.com/LinuxHello/branding)         | The brand guide, the logo and its variants                                                                             |

Start with the [architecture sections](https://github.com/LinuxHello/architecture/blob/main/arc42/README.md) and the [decision records](https://github.com/LinuxHello/architecture/blob/main/adr/README.md).

## Architecture and decisions

- The architecture is documented in the twelve [Arc42 sections](https://github.com/LinuxHello/architecture/blob/main/arc42/README.md) ([ADR-0010](https://github.com/LinuxHello/architecture/blob/main/adr/0010-arc42-architecture-documentation.md), proposed), with the C4 model in Structurizr DSL (system context, containers, components).
- The architecture, the decision records and the spikes live in their own repository, so changing them does not run the code's gates ([ADR-0011](https://github.com/LinuxHello/architecture/blob/main/adr/0011-architecture-repository.md), accepted). `linux-hello` pins a commit of it as a submodule. CI checks that every crate and component module names an element of that model ([ADR-0008](https://github.com/LinuxHello/architecture/blob/main/adr/0008-code-documentation-mapped-to-c4.md), proposed).
- A change to a container, component, interface or decision starts as a pull request in `architecture`; the code follows in `linux-hello`.

## Contributing

- Run `make next` in `linux-hello` or read [the backlog](https://github.com/LinuxHello/linux-hello/blob/main/docs/project/backlog.md) to find work. See [CONTRIBUTING.md](https://github.com/LinuxHello/linux-hello/blob/main/CONTRIBUTING.md).
- Every commit is signed off under the [Developer Certificate of Origin](https://developercertificate.org/) (`git commit -s`). The human who submits a commit signs it off; an agent never does.
- Code is largely written with coding agents, in roles fixed by [ADR-0004](https://github.com/LinuxHello/architecture/blob/main/adr/0004-agent-roles.md): a local model implements one issue at a time from a brief, Claude writes briefs and reviews, GitHub Copilot gives a second review, and the maintainer decides and merges. The same rules and checks apply whoever wrote a line ([AGENTS.md](https://github.com/LinuxHello/linux-hello/blob/main/AGENTS.md)).
- Every security property is written in plain language with a test whose name states it. Review is of behaviour, tests and test results.
- Governance and one-way decisions: [GOVERNANCE.md](https://github.com/LinuxHello/linux-hello/blob/main/GOVERNANCE.md).

## Verified builds

Proposed in [ADR-0006](https://github.com/LinuxHello/architecture/blob/main/adr/0006-attested-quality-gates-and-releases.md) and enforced by the required checks:

- A commit passes the quality gate only when every gate (fmt, clippy, tests, cargo-deny, cargo-audit, unsafe policy, fuzzing, docs) passed. CI then signs a Sigstore attestation for that commit; `make verify-gate SHA=<commit>` checks it.
- A release is built from a tagged commit on `main` whose quality-gate attestation verifies. Each file gets signed build provenance; the binaries and source archive also get a CycloneDX SBOM attestation. Verify a file with `gh attestation verify` as described in [SECURITY.md](https://github.com/LinuxHello/linux-hello/blob/main/SECURITY.md#verifying-a-release).
- Gates, attestations and release builds run on GitHub-hosted runners. [ADR-0009](https://github.com/LinuxHello/architecture/blob/main/adr/0009-self-hosted-runner.md) (proposed) adds a self-hosted runner for jobs that are not gates: a second run of the gates, long fuzzing, hardware tests and an independent release rebuild. It signs nothing.

## Security

Report vulnerabilities privately through the [Security tab](https://github.com/LinuxHello/linux-hello/security) of `linux-hello`, never in a public issue. The security model and response times are in [SECURITY.md](https://github.com/LinuxHello/linux-hello/blob/main/SECURITY.md).

## Licence

LGPL-3.0-or-later ([ADR-0002](https://github.com/LinuxHello/architecture/blob/main/adr/0002-licence-lgpl-3-or-later.md)). GPL components (KeePassXC, fprintd) are reached across a process boundary only.
