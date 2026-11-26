# Architecture — implementation baseline

## ADR-01: lightweight components

Default implementation: current supported Node.js LTS for MCP daemon, audited official MCP SDK or vetted protocol library where suitable; minimal additional runtime dependencies; native Swift/AppKit menu-bar companion (LSUIElement), local HTML/CSS/JS dashboard if helpful. A single bundled self-contained runtime or signed installer is preferred. Measure actual overhead, including runtime, before choosing rewrite (Go/Rust/Swift are candidates only if material improvement and parity/security proven). Do not introduce Electron, Docker, heavyweight databases, or a third-party hosted ChatLocal control plane by default. Supported macOS minimum to be measured/declared (initial engineering target macOS 13+; Intel not assumed tested).

## Boundaries and data flow

Cloud MCP client -> public HTTPS tunnel -> remote API listener (loopback, dedicated fixed port) -> authenticated request dispatcher -> centralized policy engine -> typed tool adapter -> selected local workspace / optional native shell worker. Results go back along the same path to the approved AI provider.

Native menu bar and local browser dashboard -> separate local control listener (different loopback port or Unix socket for app; browser loopback if needed) -> admin authentication and CSRF checks -> policy/grant store, job supervisor and tunnel manager. The remote listener must NOT mount or forward any local-admin routes, UI, native picker, logs or secrets. Do not rely on forwarded headers to distinguish remote/admin. No public exposure of the admin socket. Protect local browser session with random credentials and same-origin checks; use Keychain/0600 state as appropriate, never expose a secret in a URL or log.

### Proposed modules (names illustrative)
- core/config: typed config and versioned migrations; atomic writes, schema validation and secure storage. Separate ChatLocal data namespace, not ~/.easy-mcp.
- core/auth: OAuth discovery, registration/client metadata, consent, grant, access/refresh tokens, resource binding, revocation; use vetted library and test all code paths. No global shared master bypass.
- core/transport: negotiated MCP revisions, HTTP limits, version-aware transport, session/principal binding and cancellation. Thin adapters only.
- core/policy: deny-by-default permissions, workspace grant IDs, policy generations, request-bound approvals, expiry and revocation. Single authoritative decision point for all tools/transports.
- core/files: normalized paths, symlink and special-file defenses, stable handles, atomic writes, hash checks, bounded read/search/metadata/binary operations.
- core/execution: explicit native shell mode, child-process tracking, scrub environment, limits, job ownership, output redaction where possible. Separate worker process and minimal privileges; not a sandbox claim.
- core/network: restricted fetch and optional exact-target private network policy; DNS resolution pinning, redirect revalidation, timeouts, response limits.
- core/tunnel: provider interface (configure, start, status, stop, reconcile, probe), owned process or owned configuration, stable canonical URI, no auto provider switching.
- core/health: independent layer state, backoff, health probes, wake/network recovery, bounded logs.
- core/audit: local structured metadata; no contents, raw tokens or commands with secrets. Retention and deletion policy.
- desktop: Swift/AppKit menu, notifications, folder picker, approved local control API, login item opt-in; start/stop owned daemon; single-instance and crash recovery.
- tests: isolated fixture data, fake clocks, synthetic clients, mock tunnels, protocol conformance, real device manual test scripts.

## Service and storage
Suggested macOS data: ~/Library/Application Support/ChatLocal for config and state, ~/Library/Logs/ChatLocal for redacted diagnostics; Keychain for sensitive material where suitable. User-specific access only. Do not store secrets in environment inherited by shell child. Keep app executable, settings, auth, logs, updater, sockets and runtime directories protected by file tools even in broader scope. Do not choose home or volume root by default. Local state survives deliberate pause/restart; grants only apply to exact approved principal/resource/workspace/capability ceiling. Store workspace canonical identity, not just display label; revalidate path and mount identity. Never carry old OAuth credentials into new installation.

Use a single fixed remote port per installation; local control port/socket distinct. Collision: surface actionable error; never kill unrelated processes or silently assign a new public endpoint. Launch service without root using native process management; opt-in login item, no hidden persistence. Stop remote listener/gate first, then cancel tracked operations, stop owned tunnel and service. If tunnel disable fails, leave API closed and show warning. Tailscale/Cloudflare configuration owned by other applications must not be modified. Native background jobs and provider processes counted separately.

## API and policy contract
Remote endpoints: only selected MCP transport, discovery, OAuth flow, minimal health endpoint. Canonical external resource = explicit HTTPS origin + /mcp (not request Host or forwarded header). Local admin: status, workspace selection, permission changes, pending approval/deny, revoke, tunnel controls, diagnostics, shutdown; all admin operations authenticated, never accessible as MCP tools. Server state machine: OFF -> STARTING -> LOCAL_READY -> CONNECTING -> REMOTE_READY or DEGRADED -> PAUSED/STOPPING. Reject remote tool calls when not enabled; OAuth and health public routes expose no data. Provider-specific protocol details in MCP_INTEROP.md.

Policy evaluation for every operation: authenticated client grant -> remote enabled -> workspace still approved -> capability ceiling and current mode -> operation/path/network policy -> request-specific approval if necessary -> execution. Recheck immediately before dispatch and commit; bind approval to canonical argument digest, client, workspace, policy generation, short TTL, nonce, and consume once. Cancel queued/in-flight operations on revocation or expiry where possible. No rollback promise for effects already executed.

Errors: structured stable codes (AUTH_REQUIRED, REVOKED, REMOTE_PAUSED, WORKSPACE_NOT_APPROVED, POLICY_DENIED, APPROVAL_REQUIRED, APPROVAL_EXPIRED, VERSION_CONFLICT, LIMIT_EXCEEDED, JOB_UNKNOWN, TRANSPORT_UNAVAILABLE). Use protocol-native errors/status plus tool errors where appropriate. Never return secret values or stack traces to remote client. Error message tells user what to do in plain language; provider rendering is best effort.

## ADR-02: execution
Native shell in selected workspace is default execution implementation (when enabled), with OS-user access outside workspace and network. No mandatory isolation. A separate restricted account or OS sandbox may be optional later, only with tested guarantees. Never describe cwd or environment filtering as filesystem/network isolation. Do not expose daemon credentials to shell child by inheritance; native shell can still read same-user state. See SECURITY.md for residual risk and actual controls.

## ADR-03: tunnel
Prefer user-controlled stable Funnel hostname as candidate, then named Cloudflare as alternative. Quick tunnel temporary test only. Must verify actual macOS Tailscale CLI variant/setup and beta/limits; avoid changing unrelated Funnel routes. No publicly exposed app before authorization tests. See RELIABILITY.md.

## ADR-04: lessons from local agent systems
Adopt separation of control/execution, structured tool calls, capability/approval boundaries, process lifecycle, cancellation and recovery. Public MCP only; do not reverse engineer ChatGPT Work/Codex private protocols. Outbound-only proprietary executor protocols may be valuable design inspiration but cloud MCP clients need a supported public reachable endpoint unless their official interface changes. No provider-specific access shortcut.

## Decision log
Initial defaults above are design decisions, not measurements. Phase 0 must record exact runtime/SDK, macOS support, protocol versions, consent proof, safe file-handle approach, tunnel ownership and distribution plan with tradeoffs in DELIVERY_STATUS.md or ADR files before implementation. Security invariants may not be weakened without owner and independent review.
