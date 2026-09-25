# Development setup

PwnTV targets Linux (Wayland/Hyprland) but is developed on Windows via WSL2.

## 1. Install WSL2 with Debian

From an elevated PowerShell prompt: `wsl --install -d Debian`, reboot if
prompted, then launch "Debian" and finish the initial user setup.

## 2. Install system packages

```bash
sudo apt update
sudo apt install -y build-essential git curl pkg-config gh \
  libfontconfig1-dev libxkbcommon-dev
```

C toolchain, Git, GitHub CLI, and the font/input libraries Slint links
against.

## 3. Authenticate git and gh

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
gh auth login
```

## 4. Clone the repo inside WSL

Clone into your **Linux home directory**, not `/mnt/c/...` — the Windows
drive mount is slower and can cause file-watcher/permission issues.

```bash
cd ~ && gh repo clone pwntv/pwntv && cd pwntv
```

Open it with the [VS Code WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl):
`code .`

## 5. Install Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"
cargo --version
```

## 6. Smoke-test Slint (via WSLg)

```bash
cargo new --bin slint-hello && cd slint-hello
cargo add slint
```

Add a minimal `slint!` macro per Slint's [Rust quickstart](https://slint.dev/docs/rust/),
then `cargo run`. A window should appear on your Windows desktop via WSLg —
if it does, the toolchain is ready for PwnTV development.
