# ADR-0002: UI stack for the dashboard shell

## Status

Accepted

## Context

PwnTV needs a gamepad-first dashboard shell that runs well on a mini-PC
under a TV, plus an input daemon that turns gamepad events into navigation
and launch actions, and a session/window manager to host the shell and the
launched apps (browser, streaming clients, game launchers). We need to pick
a UI toolkit for the shell and a session/compositor approach, evaluated on:

- **Gamepad focus navigation** — how well the toolkit supports directional
  focus movement between UI elements without a mouse or keyboard.
- **Performance on a mini-PC** — low-power/low-end x86 hardware, GPU-bound
  home theater use case, needs to stay responsive and light on resources.
- **Learning curve** — how quickly a contributor can become productive.

### Options considered

1. **Rust + Slint** — a Rust-native declarative UI toolkit with a software
   and GPU renderer, designed for embedded/kiosk-style devices.
2. **Electron (JS/TS + Chromium)** — widely used for desktop apps, huge
   ecosystem, but ships a full Chromium + Node runtime per app.
3. **Qt (C++ or PyQt/PySide)** — mature, batteries-included toolkit with
   good focus-navigation primitives (`QML` focus scopes), but a heavier
   dependency and licensing surface (LGPL/commercial split) to manage.
4. **GTK4 (C or Rust bindings)** — solid on Linux, decent performance, but
   focus/gamepad navigation and kiosk-mode theming require more manual work
   than the alternatives.

## Decision

We will build the dashboard shell in **Rust with Slint**, backed by a
**Rust input daemon** that reads gamepad events and feeds them to the shell
(and to launched apps where needed), running inside a **Hyprland** session
that owns compositing, window placement, and app lifecycle. The shell, input
daemon, and any supporting crates live in a single **Cargo workspace**.

Rationale:

- **Gamepad focus navigation** — Slint's built-in focus-chain model maps
  well onto directional gamepad input, and a dedicated Rust input daemon
  gives full control over how gamepad events are translated into focus
  moves and launch actions, independent of what any individual launched app
  supports natively.
- **Performance on a mini-PC** — Rust and Slint have a small runtime
  footprint and no bundled browser engine, which matters on the low-power
  hardware this is meant to run on. Hyprland is a lightweight Wayland
  compositor well suited to a dedicated kiosk session.
- **Learning curve / cohesion** — keeping the shell, input daemon, and
  future services in one Rust Cargo workspace avoids cross-language
  bridging (e.g. Rust-to-Electron or Rust-to-Qt bindings), keeps the build
  and dependency story simple, and lets contributors work across the whole
  stack without switching languages.

Electron was ruled out primarily on performance grounds (a full Chromium
process per surface is heavy for a mini-PC), Qt on licensing/dependency
weight, and GTK4 on the amount of custom work needed for gamepad-first
focus navigation.

## Consequences

- All shell and input-daemon code is Rust, in one Cargo workspace.
- Session management (launching apps, window placement, input routing) is
  configured through Hyprland rather than a custom compositor.
- Contributors need to be comfortable with Rust; there is no JS/TS or C++
  UI layer to fall back on for the shell itself.
- Launched third-party apps (browser, streaming clients) still run as their
  own processes/windows managed by Hyprland — this ADR only decides the
  shell and session stack, not how individual services are packaged.
