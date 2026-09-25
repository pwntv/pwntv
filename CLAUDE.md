# CLAUDE.md

Guidance for AI-assisted development on PwnTV.

## Project

PwnTV is an open-source, gamepad-first home theater platform: a single
controller-driven dashboard for launching streaming services, games, and a
browser, without vendor lock-in. See [README.md](README.md) for the full
vision and current status (early development, no working dashboard yet).

## UI stack

Decided in [ADR-0002](docs/adr/0002-ui-stack.md):

- **Dashboard shell**: Rust + [Slint](https://slint.dev/)
- **Input handling**: a Rust input daemon translating gamepad events into
  focus navigation and launch actions
- **Session/window management**: [Hyprland](https://hyprland.org/) config
- All of the above live in **one Cargo workspace**

## Working conventions

- Rust is the primary implementation language across the shell, input
  daemon, and supporting crates in the workspace.
- Architecture decisions are recorded as ADRs in `docs/adr/`. Propose a new
  ADR for any decision that changes the UI stack, session model, or how
  services/apps are launched.
- Development is tracked via GitHub milestones and issues; check open
  issues before starting new work to avoid duplication.
- The project is pre-MVP: prefer small, reviewable changes over large
  speculative ones, and keep the README's "Status" section honest about
  what actually works.

## Community docs

See [CONTRIBUTING.md](CONTRIBUTING.md), [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md),
and [SECURITY.md](SECURITY.md) for contribution, conduct, and security
reporting guidelines.
