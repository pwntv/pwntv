# ADR-0001: License the project under GPL-3.0

- **Status:** Accepted
- **Date:** 2026-09-25
- **Deciders:** @<your-github-username>

## Context

PwnTV is an open-source, ad-free TV dashboard distro. Its core promise is that users own their TV: no ads, no lock-in. We need a license before accepting any code or contributions, because unlicensed code is "all rights reserved" by default.

Options considered:

| Option | Summary |
|---|---|
| GPL-3.0 | Copyleft: modified versions that are distributed must stay open source under the same license |
| MIT | Permissive: anyone may do anything, including closed-source redistribution |
| Apache-2.0 | Permissive, with an explicit patent grant |

## Decision

We license PwnTV under **GPL-3.0-only**.

## Consequences

- **Positive:** Nobody can ship a closed, ad-filled fork of PwnTV, which protects the project's core promise.
- **Positive:** It fits the Linux desktop ecosystem (Debian, most of GNOME/KDE tooling), so combining with other components is straightforward.
- **Negative:** Some companies avoid GPL code, which may limit commercial adoption of reusable parts.
- **Follow-up:** If a standalone reusable library emerges later (e.g. the gamepad input daemon), we may license that library separately in its own ADR.
- **Follow-up:** Every source file gets an SPDX header: `SPDX-License-Identifier: GPL-3.0-only`.
