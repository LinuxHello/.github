# Contributing to Linux Hello

These rules apply to every repository of the LinuxHello organisation that has no `CONTRIBUTING.md` of its own. The code repository's [CONTRIBUTING.md](https://github.com/LinuxHello/linux-hello/blob/main/CONTRIBUTING.md) and [AGENTS.md](https://github.com/LinuxHello/linux-hello/blob/main/AGENTS.md) give the full rules.

- **Where a change goes.** Code, security properties and policy semantics: [`linux-hello`](https://github.com/LinuxHello/linux-hello). Architecture, decision records and spikes: [`architecture`](https://github.com/LinuxHello/architecture). A change to a container, component, interface or decision starts there ([ADR-0011](https://github.com/LinuxHello/architecture/blob/main/adr/0011-architecture-repository.md)). Logo and brand guide: [`branding`](https://github.com/LinuxHello/branding).
- **Issues** are tracked in [`linux-hello`](https://github.com/LinuxHello/linux-hello/issues), one backlog for the organisation.
- **Pull requests only.** `main` is protected in every repository: branch, open a pull request, squash merge. The description has three sections: intent, process mapping, acceptance criteria met.
- **Sign off every commit** under the [Developer Certificate of Origin](https://developercertificate.org/): `git commit -s`. The `dco` check refuses a pull request with an unsigned-off commit. Commits on `main` are signed.
- **Name the tools** that produced a substantial part of a change with an `Assisted-by:` trailer.
- **Public repositories.** Never write secrets, tokens, personal data or details of anyone's machines into code, docs, issues, commit messages or pull requests.
- **Write plainly.** Lead with the point; no superlatives such as "bulletproof" or "fully secure".

All code and documentation is LGPL-3.0-or-later ([ADR-0002](https://github.com/LinuxHello/architecture/blob/main/adr/0002-licence-lgpl-3-or-later.md)) unless a repository says otherwise.
