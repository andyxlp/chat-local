# Acceptance criteria and release decision

Status: all NOT RUN. Mandatory gates are release blockers. A test skipped/unavailable is BLOCKED, not PASS. Product owner may explicitly change scope or targets but cannot silently waive a security invariant. Independent reviewer signs off security; owner approves actual release after Codex implementation. Requirements IDs in PRODUCT_SPEC.md, SECURITY.md, FEATURE_PARITY.md and RELIABILITY.md; evidence tests in TEST_PLAN.md.

| Gate | Acceptance criterion | Evidence | State |
|---|---|---|---|
| A01 | ChatLocal branded app/menu/CLI/package/MCP metadata/storage/installer/docs, original attribution retained, no legacy executable in package | T16 | NOT RUN |
| A02 | No default public access, no shared root, no shell/write by default, no admin/root/Docker/Electron required | T01 T10 T26 | NOT RUN |
| A03 | Zero usable unauthorized credentials or data access across every remote route/transport; local consent required and cannot be bypassed | T02–T04 T07 T18 | NOT RUN |
| A04 | Token audience/client/grant binding, expiry/rotation/revocation and restart durability; no URL secret or replay bypass | T04–T06 T18 | NOT RUN |
| A05 | Separate local admin and remote listener, no remote admin exposure/CSRF/forged proxy bypass | T07 | NOT RUN |
| A06 | No direct file-tool escape or mandatory sensitive path exposure; safe atomic/conflict writes; approved root grants | T08–T10 T17 | NOT RUN |
| A07 | Native command/job capability preserved, default per-command local approval and timed grants, read-only denies execution; cancel tracked jobs, accurate warning on residual risk | T06 T10 T11 | NOT RUN |
| A08 | No tested SSRF/DNS rebind/private network bypass or indirect Git execution; no unsafe compatibility exceptions | T12 T13 | NOT RUN |
| A09 | Request and resource abuse bounded, audit/diagnostics sanitized, no hidden upstream calls/telemetry/downloads | T14–T16 T29 | NOT RUN |
| A10 | Every mandatory F01–F28 feature implemented with tests; F29/F30 staged only according to explicit scope and documented results | T17 T18 | NOT RUN |
| A11 | Stable user-controlled public HTTPS endpoint; no URL changes on normal restart/pause/recovery; both intended stable provider adapters tested or owner-approved documented exception | T19 T20 | NOT RUN |
| A12 | Local, tunnel, reachability, auth and client activity distinguished; failure visible within 60s and controlled recovery within 120s excluding documented provider/network/sleep constraints | T19–T22 | NOT RUN |
| A13 | 8-hour and 24-hour real-device soak incl 3h idle, refresh, network/wake/restart; same connector where provider supports, no duplicate mutations, no stale green or uncontrolled memory growth | T21 T22 | NOT RUN |
| A14 | ChatGPT actual custom remote connector tested on owner's available account/interface with read/write, approvals, revoke, refresh and reconnect | T23 | NOT RUN |
| A15 | Claude actual custom remote connector tested with equivalent workflow | T24 | NOT RUN |
| A16 | Gemini actual requested web custom remote connector tested; if unsupported, blocked and owner must explicitly decide alternative scope (API/CLI is not web parity) | T25 | NOT RUN |
| A17 | Native menu bar, background operation, start/pause/resume/quit, opt-in login, owned shutdown, no unrelated process/config changes | T26 | NOT RUN |
| A18 | Actual macOS idle core RSS <80 MB and app+core <120 MB (p95), mean idle CPU <1% one core, median cold dashboard ready <3s; tunnel and packaging size separately measured; misses disclosed and owner-approved before release | T27 | NOT RUN |
| A19 | Owner can install/pair/work/approve/revoke/diagnose/uninstall with plain-language UI and no routine CLI; one-time official setup exceptions documented | T28 | NOT RUN |
| A20 | Reviewed upstream detection, verified explicit release update, no automated upstream execution, reproducible/documented build and rollback | T16 T29 | NOT RUN |
| A21 | Independent review closes all Critical/High security and P0/P1 functional/reliability blockers; residual risks disclosed; release artifact corresponds to reviewed commit | T30 | NOT RUN |
| A22 | Owner approves final device/provider compatibility, known limitations and signed/notarized distribution or explicit disclosed development-build limitation | T16 T23–T28 T30 | NOT RUN |

## Release decision
Development builds remain local-only/synthetic until security review permits controlled public testing. A release is not ready merely because code compiles, CI passes or dashboard opens. Require gate-by-gate evidence with commit hash and environment, actual provider tests, actual native app measurements and independent review. If a provider changes authentication or feature availability, mark that specific support blocked and do not lower security. No public release with unresolved P0/P1 risks. Record remaining optional enhancements, known limitations and rollback steps. Owner approval required before publishing a release or distributing a binary.

## Evidence template
Gate ID | state PASS/FAIL/BLOCKED/NOT RUN | commit | test ID(s) | date/environment | evidence link/path (redacted) | reviewer | exception/owner decision. Store results in DELIVERY_STATUS.md or linked versioned reports. Never include private data.
