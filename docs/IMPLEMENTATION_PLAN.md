# Implementation plan — Codex execution sequence

Status: approved engineering direction; no code or tests run. Implement in separate branches, small PRs, evidence per requirement. Do not publish binaries or expose owner workstation until gates pass. All phases below produce concrete artifacts.

| Phase | Scope and deliverables | Exit gate |
|---|---|---|
| 0 — Discovery and baseline | Read all docs, compare inert upstream source, create inventory and threat-model ADR; choose current supported Node LTS, official SDK/version, macOS/arch targets and test framework; investigate actual client auth/transport capabilities and current official docs. Build synthetic fixtures, local-only conformance harness and baseline measurement script; inspect 9router for lightweight UX patterns only. | G0: documented decisions, no unsupported claims, tests/harness boot locally; plan for unresolved client support. |
| 1 — Secure local foundation | Core modules, separate listeners, local admin auth/CSRF, config/storage, workspace grants and policy engine; strict OAuth local approval, tokens and revocation; safe file operations; central dispatcher; no public tunnel. | G1: S01–S10/S12–S13/S16–S20 negative/positive tests, no unauthenticated tool/consent or admin bypass; independent preliminary review. |
| 2 — Execution and parity | Native shell worker, per-command approvals, timed grants, scrubbed environment, tracked jobs, safe Git, fetch DNS pinning, security status and structured errors; all original tool capabilities. | G2: parity suite F01–F28, execution/revocation/path/network abuse tests, no unresolved high-risk defects. |
| 3 — Reliable remote | Tunnel adapters, stable canonical resource, public health, fault-injection and reconnect supervisor, auth refresh and transport version handling. Actual public test using synthetic data only with explicit operator authorization. | G3: security review before exposing test tunnel; fault tests and stable URL verified. |
| 4 — Desktop UX | Native menu bar, local dashboard, picker, approval dialogs, status, mode/toggle guidance, login opt-in, lifecycle/installer/uninstaller, verified runtime/provider onboarding. | G4: macOS UI manual tests and performance targets measured, no terminal in daily flow. |
| 5 — Integration and enhanced workflows | Actual ChatGPT/Claude/Gemini account tests, chunk/binary/pagination/atomic edit improvements, job progress, client-specific docs; tune limits. Upstream monitor and reviewed update path. | G5: client matrix evidence, all mandatory functionality, documented blockers; no insecure compatibility fallback. |
| 6 — Release validation | 8h/24h soak, sleep/wake, network failure, restart, security regression, dependency/provenance review, signed/notarized distribution as feasible, independent review, owner acceptance. | G6: all mandatory acceptance criteria PASS with evidence, no P0/P1 defects; owner explicitly approves release. |

## Work breakdown / dependencies
- Phase 0: A0 inventory + exact commit; A1 runtime/SDK ADR; A2 threat model and public interface contract; A3 synthetic fixture harness; A4 provider capability/compatibility spike; A5 benchmark scripts. Do not use the uncommitted experimental OAuth blobs as authoritative code.
- Phase 1: B0 config and separate control/remote ports -> B1 policy/store -> B2 auth/discovery/consent/revoke -> B3 protected tool dispatcher -> B4 path/file primitives and tests -> B5 local-only end-to-end. No other auth path.
- Phase 2: C0 request-bound approvals -> C1 native execution/job lifecycle -> C2 fetch/SSRF -> C3 Git safety -> C4 parity and structured output. C1 must honor B1 and cancel on revocation.
- Phase 3: D0 tunnel ownership abstraction -> D1 stable URI + provider setup -> D2 health monitor/state/backoff -> D3 transport lifecycle/refresh -> D4 synthetic public e2e. Public exposure only after G1/G2 security gate.
- Phase 4: E0 menu app/control API -> E1 picker/setup/pair -> E2 mode/approval/status -> E3 service lifecycle -> E4 build/distribution/uninstall -> E5 measurement and UX walkthrough.
- Phase 5: F0 provider matrix -> F1 real account tests -> F2 binary/chunk improvements -> F3 docs and upstream notification workflow -> F4 regression and optimization.
- Phase 6: H0 final threat review -> H1 fuzz/negative tests -> H2 soak -> H3 package verification -> H4 owner acceptance and release notes.

## Expected repo layout (implementation may justify adjustments)
src/core/{auth,policy,files,execution,network,transport,tunnel,health,audit,config}; src/cli; desktop/macos; tests/{unit,integration,security,interop,soak,fixtures}; scripts/{build,benchmark,diagnostics}; docs/adr; .github/workflows. Root package identity chatlocal, no executable until secure local alpha. Keep reference/ out of packaging, lint runtime only, never execute legacy snapshots. Use current supported SDK version and lockfile; minimize dependencies and audit transitive packages. CI permissions least privilege, pinned action SHAs, no real secrets in PRs/fork builds.

## Deliverables and review rhythm
Each PR: scope/IDs, design and changes, exact tests/commands/results, unsupported variants, dependency diff and risk assessment, rollback path. Update DELIVERY_STATUS.md. Reviewer may request changes; owner approves any security tradeoff or scope change. Use feature branches; no destructive changes to history or upstream. Create reproducible build instructions for reviewer and user. Release artifact must correspond to audited commit. Do not call a mock test real-provider evidence.

## Phase 0 handoff prompt
Read AGENTS.md and linked specs, inventory reference snapshot, decide runtime/SDK and compatibility approach with current official sources, produce a local-only synthetic test harness and ADRs, then implement secure foundation in order. Work autonomously on nonmaterial implementation choices, record them. Stop for owner if provider does not support secure required flow, signing credentials or account access are needed, or a security constraint must change. Do not use a real folder or public tunnel until explicit approval and prior gate.
