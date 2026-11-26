# Verification and test plan

All tests initially NOT RUN. Test on disposable synthetic workspaces with fake credentials. Automated tests in CI (macOS and relevant portable core environments) plus actual Mac and cloud-client manual tests. Record date, commit SHA, OS/arch, runtime/SDK/tunnel versions, exact command, expected/actual, pass/fail/blocked, redacted evidence path and reviewer. Test IDs map to requirements.

| Test | Method / expected result | Coverage |
|---|---|---|
| T01 | Fresh app install: no remote listener exposed/no shared folder, no shell or writes; no admin/root requirement; no unsolicited egress. | P01–P03 S01 S18 S19 |
| T02 | Unknown public caller attempts each data route, RPC methods, aliases, SSE and session endpoints: all denied, no file/tool/job data; OAuth registration alone yields no usable access. | S02 S06 |
| T03 | Consent flow: pending request cannot yield authorization code until exact local matching approval; deny/timeout/replay/approval of changed request fail; test malicious client display name and redirect. | S03 S04 S07 S13 |
| T04 | OAuth protocol: invalid redirect, wrong issuer/resource/audience, wrong PKCE, reused code, missing client auth, expired code, invalid grant, malformed params all fail; happy path succeeds. | S03 S05 |
| T05 | Refresh expiry, rotation and concurrent refresh/lost response; replay handling revokes affected family; persist across restart with valid grant, wrong client/resource rejected. Corrupt state fails closed. | S05 S06 R07 |
| T06 | Revoke while stream/session/job exists; subsequent calls rejected across all transports; queued work canceled; active tracked processes stopped, no false claim of rollback. | S06 S11 S20 R06 |
| T07 | Remote listener requests admin/control paths and forged Host/forwarded headers; fails independently of proxy headers. Local browser CSRF, DNS rebinding, origin and secret checks, UI escaping tests. | S07 |
| T08 | Traversal, absolute paths, symlink chain, nonexistent ancestors, hardlinks, special files, rename/race, mount/root replacement; no read/write/search/metadata outside approved roots. | S08 S09 |
| T09 | App/config/token dirs and common secrets denied including listing, search, bulk, advanced root and aliases; approved narrow exceptions only where policy permits. | S09 |
| T10 | Mode matrix Review/Edit/Run/Advanced/Off, tool aliases and cached list: read-only never mutates or runs commands; local owner controls always available; new workspace requires grant. | S10 S20 P09 P20 |
| T11 | Command/job exact approval, changed args/client/workspace/expiry/replay blocked; timer expiration and mode downgrade stop tracked jobs; env does not inherit test secrets; bounded timeout/output; native shell warning accurate. | S11–S13 F09–F13 |
| T12 | Fetch private/loopback/link-local/reserved IPv4/v6/mapped addresses, mixed DNS, rebinding, DNS failure, redirect to private, non-HTTPS/port/credentials, timeout/oversize denied; pinned public HTTPS success. Private exception exact-target only. | S14 F16 |
| T13 | Git malicious config/includes/hooks/pager/external diff/textconv/fsmonitor/filter/gitdir/symlink fixtures do not execute or read outside root; safe status/diff still work. | S15 F14–F15 |
| T14 | Malformed oversized requests, request floods, pending-registration floods, rate limits, session/job/map bounds, output caps and cancellation; approved grants unaffected by anonymous churn. | S16 R08 |
| T15 | Audit/logs/diagnostic export redaction; synthetic tokens, content and sensitive command arguments never in remote errors, logs, CI or URL. Egress capture shows only authorized tool/tunnel/update traffic. | S01 S17 |
| T16 | Supply chain/install/update: dependency pins, official binary verification, package contents exclude legacy, no updater auto-exec, app identity/storage migration, uninstaller touches only owned files/config. | S19 P14 P15 P17 |
| T17 | Every F01–F30 tool/feature has positive/negative fixture; hashes, conflict, atomic writes, batch partial result, pagination/chunk integrity, size limit. | P07 P12 P13 |
| T18 | MCP negotiated revisions, init/list/call/notifications, version header, invalid session and cancellation, structured result fallback; no bearer in URL, correct discovery/resource. | F22 F23 MCP_INTEROP |
| T19 | Fake tunnel exits, stale public probe, wrong DNS/port, auth error, provider outage: correct layered state, backoff/circuit breaker, no duplicates or unrelated process kill. | R01–R06 R10 |
| T20 | Actual stable tunnel from independent external vantage; same public origin after restart, pause/resume, no unauthorized call; no new connector required for recoverable transport loss where provider supports. | P04 P05 R01–R06 |
| T21 | 8h soak with 3h idle, forced refresh, lost network and tunnel restart; record real endpoint, verified client calls, recovery time; no stale green or unexplained failure. | R07–R12 |
| T22 | 24h soak including wake/network transitions, process crash/restart and bounded session/job usage; check actual existing connector before/after, no unbounded RSS trend. | R07–R12 |
| T23 | Actual ChatGPT custom MCP account: OAuth, read/write, denial/approval/shell, job polling, revoke, refresh, cached tools and reconnect. | P18 |
| T24 | Actual Claude custom connector: same matrix, identity and redirect behavior. | P18 |
| T25 | Actual Gemini web custom MCP account/interface: same matrix; API/CLI alternative separately named, not evidence for web. | P18 |
| T26 | Native menu lifecycle: launch/close dashboard, pause/resume, quit, crash, login opt-in/out, multi-instance, port collision, provider shutdown failure; no hidden persistence, no other services touched. | P01–P03 R06 |
| T27 | Performance benchmark actual Apple Silicon Mac: cold/warm readiness, 10min idle CPU/RSS core/app/tunnel separately, typical file reads/search/edits/commands, package size, peak memory and soak trend. | P16 |
| T28 | Nontechnical owner completes install, official tunnel setup, pair, select folder, Review/Edit/Run, approve/deny, diagnose, revoke and uninstall; record actual steps/time/errors and feedback. | P02 P09 P10 P17 |
| T29 | Upstream monitor observes changed pinned commit, reports metadata/diff without running code; no runtime author callback; simulated update tamper rejected, rollback docs. | P14 P19 |
| T30 | Independent security reviewer traces routes, auth, file primitives, command worker, network and build chain; verify high findings closed and remaining native-shell/provider risks documented. | All S / release |

## Soak protocol
Test on representative owner's macOS hardware or equivalent and disclose differences. Use only synthetic folder, test account/grant and owner-authorized tunnel. Stable endpoint, actual client connector already configured. Baseline record process IDs/versions, URL fingerprint (not live URL in public logs), active grant, CPU/RSS, reachability. Idle at least three hours without tool calls, then read/write safe fixture. Repeat after token TTL/refresh, deliberately stop only ChatLocal-owned tunnel, toggle Wi-Fi/offline, sleep/wake, restart daemon, pause/resume. Verify public failure within 60s, recovery within 120s after network restored in controlled tests, no silent new endpoint, no duplicate mutation, no false online. Capture client error separately from public probe. 24h test includes multiple cycles and resource trend, not just keeping process open. Unavailable test must be BLOCKED.

## Benchmarks
Use OS tools or instrumented harness; record hardware, OS, Node/SDK, tunnel type, active jobs, sampling interval, cold/warm counts. At least 5 cold starts; report median/max; at least 10 min steady idle, report median/p95 RSS and mean CPU as percentage of one core; separately report app, core, tunnel and total. Measure app package size and compare to legacy local-only baseline only in isolated test environment after security review, never execute insecure public server. No invented benchmark. If target misses, profile and justify revision or obtain owner approval before release.

## Evidence handling
Commit only sanitized test reports and synthetic fixtures. Raw logs and screenshots stay local, remove tokens, paths, account identifiers, live hostnames and file content. Independent reviewer receives redacted evidence. CI pass alone cannot prove provider compatibility, native packaging, or real uptime.
