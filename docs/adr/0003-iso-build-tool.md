# ADR-0003: Build the PwnTV image with live-build

- **Status:** Accepted
- **Date:** 2026-09-25
- **Deciders:** @dev-494
- **Issue:** #5

## Context

PwnTV ships as a Debian 13 "trixie" based image containing Hyprland, the Rust + Slint dashboard shell, the Rust input daemon and their configuration (ADR-0002). Users should be able to flash it to a USB stick, boot it on an x86_64 mini-PC, try it live, and install it.

We need a tool that builds this image reproducibly, locally (WSL2) and in CI (GitHub Actions), so every release can ship an image.

Options considered:

| Option | Output | Config style | Notes |
| --- | --- | --- | --- |
| **live-build** | Hybrid live ISO, optional installer | Folder of package lists, included files and hook scripts | Debian's own tool; used by Debian Live and Kali Linux |
| mkosi | Disk images (appliance-style); `mkosi qemu` for fast boot tests | Declarative INI | Modern, fast iteration; installer ISO is not its focus |
| debos | Disk images, mostly ARM/embedded | YAML recipes | Prefers KVM; better fit for ARM boards |

## Decision

Use **live-build** to produce a hybrid ISO for x86_64 (BIOS + UEFI).

Repository layout (under `image/`):

- `config/package-lists/*.list.chroot`: Debian packages to install (Hyprland, Chromium, etc.)
- `config/includes.chroot/`: files copied into the image (Hyprland config, systemd units, plugin manifests)
- `config/hooks/live/*.hook.chroot`: scripts run inside the image during the build
- `auto/config`: the `lb config` options, so the build is one repeatable command

Rust binaries are built separately (`cargo build --release`) and copied in via `includes.chroot`, or packaged as `.deb` files later.

## Consequences

- **Positive:** Produces the format users expect (flash → try live → install), with lots of prior art to learn from.
- **Positive:** The folder-based config is easy to read and review in pull requests.
- **Positive:** Runs in WSL2 (Debian, with `sudo`) and in GitHub Actions (Debian container with `--privileged`), so CI can attach an ISO to every release.
- **Negative:** Builds are slow (minutes, not seconds), and the tool has dated, sometimes cryptic defaults.
- **Negative:** Needs root and loop devices; CI needs a privileged container.
- **Mitigation:** For the fast inner loop, the shell and daemon are developed and tested outside the image (WSL2); the ISO is only built for integration tests and releases.
- **Revisit:** If PwnTV moves to an appliance model without an installer (like SteamOS), reconsider mkosi. The image config (packages, files, hooks) would largely carry over.
