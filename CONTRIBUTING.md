# Contributing

Thanks for your interest in contributing to pwntv!

## Workflow

1. **Issue first.** Open or claim a GitHub issue describing the change before writing code. This keeps work visible and avoids duplicate effort.
2. **Branch.** Create a branch named `<type>/<issue#>-<slug>`, e.g. `feat/12-dashboard-charts` or `fix/8-login-crash`.
3. **Commit.** Use [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `chore:`, etc.) for commit messages.
4. **Pull request.** Open a PR using the repository's PR template, and link it to the issue it closes (e.g. `Closes #12`).
5. **Merge.** PRs are merged via **squash merge** to keep `main` history linear.

## Commit types

Common types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`, `build`, `perf`.

## Code headers

New source files should include an [SPDX license identifier](https://spdx.dev/) header at the top, e.g.:

```text
// SPDX-License-Identifier: MIT
```

## Architecture Decision Records

Significant architectural or design decisions are recorded as ADRs in [docs/adr/](docs/adr/). If your change introduces or changes an architectural decision, add a new ADR following the existing numbering convention.

## Getting help

If anything here is unclear, open an issue or start a discussion — we're happy to help.
