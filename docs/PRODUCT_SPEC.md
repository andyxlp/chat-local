# Product specification — ChatLocal v1

Baseline: 2026-11-26. Normative requirements use MUST; SHOULD means justified exceptions recorded. Status: ready for engineering, not release. Owner is a nontechnical operator; Codex builds, independent reviewer validates.

## Problem and goals

The existing Easy MCP utility is easy and light but its dashboard can report local uptime even if remote connectivity fails. The owner observed repeated ChatGPT failures after roughly 2–3 hours and had to recreate connectors using new endpoints. The precise cause has not been established. The owner wants approved ChatGPT, Claude and Gemini agents to read/edit local project material and optionally execute native commands; the original author and unapproved third parties must not gain access merely because a tunnel exists. ChatLocal is a tool bridge, not a model or a replacement subscription service.

## Personas and priorities

One primary Mac user, working on research, spreadsheets, documents, software and local automation, not just coding. Wants a one-time installation, simple everyday control, readable risk explanations, and no routine terminal use. Priority order: P0 owner-controlled security; P0 reliable actual cloud connectivity; P0 feature parity and native execution; P0 simplicity and low overhead; P1 improved large-file/task workflows; P1 reviewed upstream updates. Tradeoffs must preserve all P0 needs.

## Requirements

| ID | Requirement | Priority |
|---|---|---|
| P01 | macOS-first native menu-bar application and background service, no mandatory container/VM/Electron, no admin/root requirement for normal usage | P0 |
| P02 | Install via signed/notarized app distribution where feasible, guided official tunnel setup, folder chooser, copy endpoint, connect instructions; no terminal for normal use | P0 |
| P03 | Start, pause remote access, resume, stop and quit, independently from dashboard/browser; opt-in launch at login only | P0 |
| P04 | Connect approved cloud clients from public internet, prefer stable endpoint; support Tailscale Funnel and named Cloudflare Tunnel (quick tunnel optional dev only) | P0 |
| P05 | Show local server, tunnel, public reachability, authorization and recent client activity separately; recover from network change, sleep/wake, tunnel failure and restart without changing endpoint where provider supports | P0 |
| P06 | One local approval per new client grant, revocation and global pause, clear client identity and workspace boundaries; no secret in URL | P0 |
| P07 | All original file/git/job/fetch capabilities supported in appropriate modes, with safer semantics where original was unsafe | P0 |
| P08 | Native command execution selectable, timed elevation and exact local approval for dangerous operations; no mandatory Docker; clear distinction from OS sandbox | P0 |
| P09 | Read-only, shell, background jobs, folder protection, public fetch, private-network access and remote access toggles with understandable consequences; policy enforced server-side | P0 |
| P10 | MCP instructions, tool status and machine-readable risk/approval messages instruct client to explain risk and wait; never depend on provider rendering or compliance | P0 |
| P11 | Local activity history with client, operation, time, outcome and approval status, not file content/credentials; export sanitized diagnostic report | P0 |
| P12 | Chunked text/binary access, search pagination, file hashes, atomic writes and expected-hash conflicts; user-friendly structured errors | P1, core safe writes P0 |
| P13 | Long-running jobs, progress/status/cancel, bounded output, no hidden rerun of writes/commands | P0 |
| P14 | No hidden telemetry, no original-author callback or auto-download, no runtime upstream dependency; opt-in reviewed update notifications | P0 |
| P15 | Stable product identity ChatLocal across app, menu, CLI, package, protocol metadata, installer, support, settings and data; keep upstream attribution separately | P0 |
| P16 | Target idle core RSS <80 MB, combined app+core <120 MB, steady idle CPU <1% of one core, local dashboard ready <3s; measure tunnel separately, publish failures rather than invent numbers | P0 |
| P17 | Installer/uninstaller clearly explains data deletion vs retention, migration from original Easy MCP is opt-in and never imports insecure auth tokens | P0 |
| P18 | Verified compatibility with ChatGPT, Claude and Gemini on actual available custom-MCP interfaces/account tiers, no unsafe bypass for unsupported feature | P0 |
| P19 | Upstream feature detection and safe merge/adaptation, versioned releases, reproducible or documented build, release verification | P1 |
| P20 | User can explicitly change workspace with visible client grants; default deny access to newly selected workspace until locally authorized | P0 |

## User flows
1. First launch: app is local-only, no folder auto-shared. Choose a folder (never home/root by default); Review mode; guide install/sign-in to official tunnel provider; show public exposure and data path; enable remote only on explicit action.
2. Pair: copy stable /mcp URL, configure custom MCP in AI product, see exact pending request locally with issuer/redirect hostname and non-spoofable verification context, approve only matching request. Browser authorization completes; test basic connection.
3. Work: choose Review (read only) or Edit (writes); agent requests permission for shell/jobs or higher-risk operations; user sees plain-language local dialog with exact action, path/client, expiry; deny or approve once/session as allowed. Permission state visible in menu.
4. Disconnect: Pause blocks data/tools immediately and cancels tracked work; stop tunnel safely; resume preserves endpoint and existing valid grants unless revoked. Quit stops owned service/tunnel/jobs; optionally keep service running only via explicit choice.
5. Diagnose: status explains which layer failed, last successful call, safe retry, and whether reauthorization is actually required. Do not promise a green badge proves all providers are connected.
6. Update: show version and reviewed ChatLocal release notes; user initiates verified install. Upstream detection never runs upstream code.

## Scope boundaries
macOS first (Apple Silicon primary; Intel support tested or explicitly limited). Windows/Linux are later targets, not silently claimed. No VPN-only default for cloud services. No scraping browser cookies, account session hijack, model proxy, private API reverse engineering, quota circumvention, mandatory paid service, remote desktop automation, general LAN discovery, privileged daemon or arbitrary unauthenticated execution. No guarantee of access during Mac sleep/offline. No promise that every cloud UI can display approvals or transfer binaries. Do not assert parity with proprietary Work/Codex local integration.

## Success definition
A nontechnical owner can install and use the actual app, pair once, switch permissions, work safely and recover from common interruptions without repeatedly creating connectors. Must pass the explicit gates in ACCEPTANCE_CRITERIA.md. All baseline metrics and provider tests are unmeasured until implementation.
