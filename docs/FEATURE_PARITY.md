# Feature parity and tool contract

Original baseline: Easy MCP v2.3.1, server.js at commit 2c94bed8be2594b0759e9bee04b62b205c564fe4. See inert reference snapshot. Preserve user capabilities, not unsafe legacy behavior. All tools must be reachable through the same centralized policy dispatcher; never grant permissions through tool listing alone. P07/P12/P13.

| ID | Original tool or feature | ChatLocal required behavior |
|---|---|---|
| F01 | read_file | Bounded text read, line/byte ranges, encoding detection, structured metadata; legacy path input supported where safe. |
| F02 | read_files | Bounded multi-file read with individual status and overall cap; no partial disclosure of denied paths. |
| F03 | write_file | Create or replace within approved root, atomic commit, expected hash for overwrite, no special files; explicit conflict. |
| F04 | write_files | Bounded batch with preflight validation, per-file result and documented partial-failure behavior; no falsely claimed transaction. |
| F05 | edit_file | Exact unique text replacement; expected hash, atomic write, conflict and preview; preserve encoding/line endings where feasible. |
| F06 | list_directory | Approved entries only; pagination, type/size, no credential-path disclosure, no symlink traversal by default. |
| F07 | search_files | Bounded recursive text search, pagination, exclusions and result limits; no unapproved paths, no execution of file content. |
| F08 | get_file_info | Approved metadata, distinguish regular file/directory, size/time/hash where practical; reject special files. |
| F09 | execute_command | Native user shell when explicitly allowed, default per-command approval, timeout/output cap, scrub env and track process tree; no sandbox claim. |
| F10 | start_job | Explicit job and shell grant, async job ID, owner/workspace/expiry, bounded output, process tracking. |
| F11 | job_status | Own job only, state/exit code/output cursor, redacted limits; no cross-client leakage. |
| F12 | stop_job | Own tracked job or local admin, cancel process group, report actual outcome. Read-only remote may not mutate jobs; local owner can always stop. |
| F13 | list_jobs | Own jobs only; bounded/paginated; local dashboard can view all metadata. |
| F14 | git_status | Safe read-only Git inspection in approved repo; no indirect execution, hooks, config abuse or path escape. |
| F15 | git_diff | Safe diff, no external diff/textconv/pager, optional approved path; bounded output. |
| F16 | fetch_url | Restricted public HTTPS fetch when enabled; explicit private destination exception only if implemented and tested. |
| F17 | get_current_project | Only approved workspace display/ID; no outside roots or admin config. |
| F18 | set_project | Preserve workspace switching via local picker; optional remote request proposes existing approved workspace or requests local grant, never expands scope. No raw remote set-project capability. |
| F19 | Dashboard + native folder picker | Native menu bar/local dashboard with recent workspaces, security modes, status, jobs, approvals, diagnostics. |
| F20 | Local launcher + CLI options | User-level app, start/stop/status controls, documented CLI for troubleshooting; no auto-killing arbitrary processes or inherited legacy flags. |
| F21 | Tunnel modes | Local-only, Tailscale Funnel, named Cloudflare, optional temporary Quick Tunnel debug. Stable URL for production. |
| F22 | MCP JSON-RPC / Streamable HTTP | Negotiated protocol versions; legacy SSE conditional on verified secure client need, not insecure parity. |
| F23 | OAuth | Replace original auto-authorization; secure locally approved grants, resource binding, revocation/rotation. |
| F24 | Activity/uptime/recent jobs | Local bounded audit and independent reachability, not misleading uptime. |
| F25 | Read-only, shell, folder jail | Preserve user-facing choice with strict policy semantics and warnings; optional scoped broad roots, never remote self-escalation. |
| F26 | Upstream update notice | Safe reviewed upstream changes, not runtime author callback or auto-install. |
| F27 | New get_security_status | Current effective permission ceiling/mode, risk, expiry and approved workspace, no secrets. |
| F28 | New request_approval / approval_status | Create/read own pending request, exact action, expiry, no self-approval; retry once after approved. |
| F29 | New paginated/binary workflow | Metadata and hash, bounded chunks with explicit encoding, verified write assembly; never arbitrary huge base64 payloads. Use local computation for large documents where appropriate. |
| F30 | New structured results and job progress | Stable machine-readable codes and text fallback; jobs status/cancel, no blind mutation retry. |

## Initial interface decisions
Use schema-validated arguments and structured result plus text fallback; stable tool names for cached client tool lists. For read_file accept path, optional start_line/end_line or byte offset/limit; reject conflicting selectors. For writes accept path, content, expected_sha256 or explicit create-only; reject overwrite without expected hash unless locally approved, return new hash/size. Multi-file calls report each outcome. No remote deletion capability assumed from original; add safe deletion only under a separately reviewed feature with local confirmation. Keep old read/write tool names if safe; incompatible changes documented with examples.

Default bounds to validate in Phase 0: max request body 8 MiB, individual text result 256 KiB, batch result 512 KiB, binary chunk 128 KiB, max 25 files per batch, search results 100/page, traversal depth 20 and entries 10k/request, shell output 256 KiB retained with cursor/truncation flag, foreground command 120s default/600s cap, jobs max 4 per client/8 global and 60 min default max lifetime (local extension allowed). Limits configurable locally with documented ceilings, never by remote AI. Choose values from actual provider limits and benchmarks; no silent unlimited operation. Consider streaming only where negotiated protocol supports it.

## Parity exclusions by design
Do not reproduce auto-approved OAuth, anonymous/tool access, master bearer in URL, unsafe shell claims, public admin routes, unsafe fetch, shell-based git inspection, unverified binary downloads, original author promotion/analytics, kill arbitrary processes, account/subscription bypass. No promise that cloud clients can directly transfer arbitrary binaries through their chat UI. Windows/Linux launchers remain reference only for v1; build later if requested. All F01–F28 mandatory for supported macOS v1, with conditional protocol/private-network variants as defined in specs. F29 enhanced binary/chunk features may be staged only with owner-approved scope exception; F30 core job status mandatory.
