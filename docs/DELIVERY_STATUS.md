# Delivery status and decisions

Handoff baseline: 2026-11-26. This repository contains a product/engineering specification and inert upstream reference only. No production or development ChatLocal runtime has been built, run, security-tested, signed or released. Earlier experimental security blobs/branch are not implementation baseline. All acceptance gates A01–A22 and tests T01–T30 are NOT RUN.

## Engineering decisions (initial, confirm in Phase 0)
| Decision | Rationale / status |
|---|---|
| macOS-first, native menu bar | Owner uses Mac and wants lightweight tray/background UX; exact OS/Intel support unverified. |
| Node.js core initially + Swift/AppKit | Retain existing tool behavior with minimal overhead; choose current LTS and vetted MCP SDK after spike; rewrite only with measured benefit. |
| No mandatory Docker, VM or Electron | Owner prioritizes lightness and native commands; residual same-user shell risk explicitly accepted as functionality, not unqualified safety. |
| Stable Funnel candidate, named Cloudflare alternative | Owner permits engineering choice; confirm actual setup/compatibility and limitations; Quick Tunnel dev only. |
| Remote off, Review default, local approval | Security-first defaults with accessible toggles; per-client ceilings, per-command approval, time-limited grants. |
| Independent local control listener and standard MCP | Separate admin from remote ingress, avoid proprietary protocols and hidden access. |
| Performance targets and soak thresholds | Initial engineering acceptance targets, not measured claims. |
| Upstream repository monitoring, reviewed updates only | Preserve useful features without granting upstream runtime trust. |
| Public fork remains public unless owner changes settings | Do not put any private files, credentials, live URLs, identifiers or logs in repo. Repository rename requires settings operation; source and product identity are ChatLocal. |

## Evidence gaps / open decisions
- Actual macOS architecture/version, tunnel installation availability, signing/notarization credentials and distribution method.
- Actual account tier/custom connector availability and supported auth/transport revisions for ChatGPT, Claude, Gemini web. No provider tested yet.
- Exact cause of observed 2–3h failures unknown; stale dashboard defect is source-confirmed only.
- Runtime/SDK/library choice, measured CPU/RSS/startup/installer size, OS-level file race defense and safe Git inspection strategy.
- Confirm token TTL/rotation behavior against real clients; no insecure exception.
- Need actual local hardware, test accounts and owner consent for public synthetic tunnel, real provider and soak tests. No production folder for tests.
- Repository slug may remain easy-mcp if connector cannot rename it; owner can rename in GitHub Settings. GitHub fork relationship and MIT history remain intentionally preserved.

## Phase status
| Phase | Status | Evidence |
|---|---|---|
| Specification/handoff | COMPLETE (documentation only) | README, AGENTS, linked docs and reference snapshot |
| 0 Discovery/baseline | NOT STARTED | — |
| 1 Secure foundation | NOT STARTED | — |
| 2 Execution/parity | NOT STARTED | — |
| 3 Connectivity | NOT STARTED | — |
| 4 Desktop | NOT STARTED | — |
| 5 Interop/enhancements | NOT STARTED | — |
| 6 Validation/release | NOT STARTED | — |

## Update template
Date / agent / branch / commit / phase / requirement IDs / files changed / commands and results / exact environment / failed or blocked tests / residual risks / ADR or owner decision / independent review status. Attach only redacted evidence. Mark each gate PASS, FAIL, BLOCKED or NOT RUN. A documentation commit is not implementation evidence.
