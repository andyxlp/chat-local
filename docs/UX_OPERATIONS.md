# UX, controls and operator guide

Design goal: no command-line or networking knowledge needed for daily work. All text in plain language; no dark patterns, hidden background service or misleading green status. Native macOS menu bar app first, local dashboard for details. No mandatory Docker, Electron, paid domain or separate backend.

## Menu bar
Icon states: gray Off, blue Starting, green Reachable/Connected (distinguish verified last call), amber Degraded, red Blocked. Text always accompanies color. Menu: start/resume, pause remote, stop/quit, current workspace/choose folder, mode, pending approvals, authorized clients/revoke, last connection status, open dashboard, diagnostics, check reviewed updates, start at login (opt-in). Close dashboard leaves service running; Quit stops owned daemon/tunnel by default. Show actual state, not just button selection.

## First run
1. Install trusted ChatLocal app; no automatic remote start. Show what files are shared and that approved cloud AI receives returned content.
2. Choose workspace with native picker (explicit confirmation for broad folders). Start in Review. No accidental sharing of entire home, drive, app data or credentials.
3. Guided stable tunnel choice: recommend Tailscale if supported; explain public URL and account prerequisites, launch official install/sign-in as user-approved step. Alternative named Cloudflare requires user-controlled domain/config. Verify provider status and canonical endpoint. No background download/execution of unverified binaries. If unavailable, remain local-only with actionable fix.
4. Enable remote connection explicitly; copy URL without secrets; show provider-specific current connection instructions. Pair with local approval correlated to browser request. Display actual redirect origin, verified identity vs claimed name, one-time matching context, expiry and effective access. Deny unexpected request.
5. Run test read of synthetic sample; label Connected only after actual call. Explain revoke/pause controls and normal use.

## Mode presets

| Mode | Read | Write | Shell | Jobs | Folder protection |
|---|---|---|---|---|---|
| Off | Remote denied | Denied | Denied | Denied | On |
| Review | Approved workspace | Off | Off | Off | On |
| Edit | Approved workspace | On | Off | Off | On |
| Run | Approved workspace | On | Timed grant | Separate opt-in | On for file tools, NOT shell |
| Advanced | Explicit additional approved roots | Per grant | Separate | Separate | Narrow scope exception, protected paths still denied |

Remote access, modes, public fetch, private network, jobs, sensitive exceptions and login start are independently visible. Never represent read-only + native shell as safe: selecting Review disables shell/jobs and cancels tracked work. Effective permissions = client grant ceiling intersect current mode; changing global mode cannot silently expand a client's ceiling. Selecting new folder requires explicit grant. App storage and mandatory sensitive paths protected even in Advanced direct file tools.

## Copy for controls
- Remote access: "Turn on to let your approved cloud AI clients connect. Your public address alone is not permission. Turn off when not using it."
- Read-only: "Use for reviewing or researching files. AI cannot change files or run commands in this mode."
- Write access: "Use when creating or editing files in this folder. Changes can overwrite work; review conflicts and backups."
- Shell: "Use for Python, tests, scripts or builds. Commands run with your normal Mac account permissions and can access files outside this folder and the network. Enable temporarily, only for a task you trust." Default 15 minutes, per-command approval, prominent countdown, revoke button.
- Jobs: "Use for longer computations or dev servers. Jobs may keep running until stopped or expired. Tracked jobs are stopped on pause/revoke; detached processes may survive."
- Folder protection: "Keep on. To work in another folder, add that folder explicitly rather than sharing your whole Mac. Shell is not constrained by this switch."
- Public fetch: "Allow fetching public HTTPS documentation. Private addresses remain blocked."
- Private network: "Only enable for a specific local service you recognize. This does not restrict native shell networking." Require exact approved destination; never allow admin or credentials.
- Client revoke: "Immediately blocks future access by this connection, including open sessions. Does not undo completed changes or data already sent."
- Pause: "Disconnect remote access without deleting your connector. Some providers may require sign-in again."

## Approval flow
AI receives structured APPROVAL_REQUIRED with reason and request ID, tells user what operation needs permission and why; local notification/menu shows exact action, client, workspace, affected paths/destination, time limit, and risk. Full command visible locally, not in log. Approve once / deny / cancel; optional timed session grant requires separate explicit confirmation. If dismissed/expired, no action. AI cannot approve from chat and cannot elevate itself. Retry exact approved action once, status query idempotent. If client fails to explain, local warning still mandatory. Never claim provider chat renders a particular widget.

## Status/diagnostics
Layered status with last check and last successful client call; no green when public URL stale. Buttons: retry connection, open provider sign-in, reauthorize only if needed, stop, copy sanitized diagnostic report (preview/redact). Explain sleeping/offline. No auto-upload. Show local recent activity metadata, approval/revocation history and job stop controls; retention configurable, default 7 days / bounded size, delete history option. Hide raw secrets and content.

## Packaging/maintenance
Signed/notarized macOS app where feasible, user-level installer, no root or Full Disk Access by default. Bundle verified runtime or guided one-time official install. On uninstall offer retain or delete ChatLocal-only config/grants/logs; no delete project files or unrelated provider config. Explain stopping old Easy MCP and not importing its tokens. Reviewed upstream update notices and explicit update action; no silent code changes. Add accessible labels, keyboard navigation, large-enough text, no essential status conveyed only by color.

## Usability acceptance
Owner completes installation, folder selection, stable tunnel setup, pairing, read/edit, denied shell, local approval and run, revoke, pause/resume, diagnostics and uninstall using UI instructions alone (no terminal after installation). Record actions/time/errors and owner feedback. If official tunnel setup requires one-time CLI intervention, provide guided copy/paste fallback and document exception; daily usage remains menu-based.
