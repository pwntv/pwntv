# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

PwnTV — an open-source, ad-free TV dashboard distro. Debian 13 + Hyprland, gamepad-first UX. Licensed GPL-3.0-only.

## Status

M0 (foundations). No code yet. The UI stack and the ISO build tool are both still undecided — check `docs/adr/` for the decision record once one exists before assuming either.

## Repo layout

- `README.md` — one-line project description
- `LICENSE` — GPL-2.0 text (see Conventions below re: SPDX header mismatch)
- `docs/adr/` — Architecture Decision Records (currently empty)
- `.github/ISSUE_TEMPLATE/` — bug report, feature request, and task issue forms
- `.github/pull_request_template.md` — PR template

## Conventions

- Commit messages: [Conventional Commits](https://www.conventionalcommits.org/) (e.g. `feat: add gamepad remapping`).
- Branch names: `<type>/<issue#>-<slug>` (e.g. `feat/42-gamepad-remap`).
- Merge strategy: squash merge into main.
- Every source file starts with an SPDX header: `SPDX-License-Identifier: GPL-3.0-only`. Note: the repo's `LICENSE` file text is currently GPL-2.0 — flag this mismatch rather than silently picking one if it matters for a task.
- ADRs live at `docs/adr/NNNN-title.md` (zero-padded sequence number + kebab-case title).

## Working rules for Claude

- Work on one issue at a time.
- Touch only the files that issue actually needs — no drive-by refactors or unrelated cleanup.
- Ask before adding a new dependency.
- Keep explanations short. The user is learning: briefly explain non-obvious choices (why this approach over an obvious alternative), but don't narrate routine or self-evident steps.
