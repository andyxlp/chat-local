# Instructions for ChatLocal engineering agents

This is the owner's approved direction and engineering handoff dated 2026-11-26. It is not implemented or release-approved. Codex is the primary implementation agent; an independent reviewer and the owner approve release. Follow the root README and all linked specifications. Security requirements and release gates override convenience; raise contradictions in docs/DELIVERY_STATUS.md rather than silently weakening them.

## Work contract
- Start from the default branch handoff, not the earlier security/hardened-access experiment. Original source is read-only reference, not a runtime dependency. Do not execute, install, serve, or copy its insecure authorization flow. Git history is preserved.
- Build in a feature branch, small reviewable commits, tests with each phase. Do not merge or release production binaries without owner/reviewer approval. Do not fabricate tests, resource measurements, provider compatibility or actual device validation.
- First inspect the current stable MCP SDK/spec and each provider's documented custom connector requirements. Pin versions and lock dependencies; use vetted implementations for authentication/transport rather than improvising OAuth. No automatic updates from upstream, no external analytics, no hidden access.
- Retain feature parity and safe user intent, not vulnerable wire behavior. Use server-side policy checks for every route/transport/tool, including aliases and queued work. AI messages/tool annotations never substitute for authorization.
- No public tunnel during development without explicit owner permission. Use temporary synthetic workspaces, fake secrets, local fixtures, mock tunnels and test accounts. No real user files, credentials or production endpoints in Git, CI, screenshots or logs. Never ask the owner to paste secrets into chat or issues.
- Native shell is intentionally supported as an elevated OS-user capability. Never claim it is confined by a working directory or read-only file guard. Default remote off, read-only on, shell/jobs/private-network off, file protection on. No mandatory VM, Docker, Electron, or admin privileges.
- Prefer simple architecture; measure and justify changes to language, framework, dependencies or threat model via ADR. Do not add service subscriptions or require a paid domain as a default without approval.
- Stop on any security P0/P1 or test failure in a release path. Do not implement unsafe fallback auth for client compatibility. If a provider cannot connect securely, record blocked with exact evidence and safe alternatives.
- Keep docs and tests in sync; identify each requirement and test ID in PRs. Update docs/DELIVERY_STATUS.md with actual commits, commands, pass/fail/blocked, environment, evidence and residual risks. All nonlocal/manual tests are initially NOT RUN.
- Preserve MIT copyright/license and attribution. Do not rename a GitHub repository or change permissions/visibility automatically; owner controls those settings.

## First engineering task
Implement Phase 0 (baseline inventory, runnable test harness, protocol/client compatibility spike and architecture decisions) from docs/IMPLEMENTATION_PLAN.md. Then implement the phases in order. Provide a working, tested local alpha before attempting a public tunnel. No claims that this handoff itself contains application code.

## Definition of done
Every mandatory acceptance criterion has traceable evidence, the independent security review is signed off, actual macOS installer/menu-bar lifecycle is tested, the owner verifies each supported cloud client, and known limitations are documented. A failing or unavailable test is not a pass. See docs/ACCEPTANCE_CRITERIA.md.
