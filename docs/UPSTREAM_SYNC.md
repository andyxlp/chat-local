# Upstream maintenance and release updates

The original Easy MCP may gain useful features. ChatLocal should benefit without making the original author a trusted operator or automatically running their code.

## Upstream identity and baseline
Repository: https://github.com/mrkaylabs/easy-mcp
Last reviewed starting snapshot: 2c94bed8be2594b0759e9bee04b62b205c564fe4 (v2.3.1, 2026-11-23 commit date). This is the comparison baseline, not a statement that it is the latest upstream version. Retain MIT license, copyright and attribution.

## Detection mechanism (implement in Phase 5)
Prefer GitHub Actions or a repository-side scheduled check, not a startup call from the user's Mac to the upstream author. Opt-in enablement by owner; default proposed weekly and manual dispatch. Public GitHub API metadata only (upstream repository identity, latest default-branch commit SHA/release tag/date); record reviewed SHA in a versioned baseline manifest. No cloning, running, installing or building new upstream code in the monitor. Treat upstream data as untrusted text, escape it in notifications; do not evaluate content as shell or workflow input. Least privilege: contents read; issues write only in separate dedup notification step if needed, no secrets, pinned action SHAs, no pull_request_target executing untrusted code. Fork schedules may need explicit enabling; validate.

When upstream SHA differs, create/update one issue labeled upstream-review or equivalent owner-visible report with old/new SHA, canonical compare link, date and review status. Deduplicate by new SHA, avoid notifying repeatedly, record failure/rate-limit separately. No auto-merge, no auto-PR containing unreviewed source. Owner can see update notification in ChatLocal via optional check of **ChatLocal's own reviewed release metadata**, not direct upstream runtime callback. Provide manual Check for updates, optional periodic checking, no telemetry or personal metadata. Display only release authenticity/version status.

## Review workflow
1. Compare exact old/new upstream commits; inventory changes to tools, auth, tunnels, UI, dependency tree, binaries, outbound destinations, installer and update paths. Read diffs in isolated environment, not execute them.
2. Classify useful additions, fixes, incompatible changes, security-sensitive changes, and upstream behavior deliberately excluded by ChatLocal. Link F/S/R/P requirements and risk assessment.
3. Adapt selected changes manually to ChatLocal architecture; don't blindly merge or copy insecure behavior. Preserve owner-approved policy/approval boundaries and identity. Use a feature branch with tests, dependency review and independent security review for sensitive changes.
4. Record review report, new baseline SHA, accepted/rejected/deferred change list, rationale, tests, release notes. Close notification only after disposition; keep traceable mapping. No upstream data/secret access.
5. Build from audited ChatLocal commit, verify package contents, publish versioned signed/notarized artifact/checksum and source provenance when feasible. Owner explicitly installs; no silent update or arbitrary remote code. Rollback to last reviewed compatible version; state migration backup, never restore insecure credentials.

## Acceptance
T29: fake new upstream commit creates deduplicated notification without executing source; simulate malformed metadata, API failure, tampered release or checksum, reject safely. Confirm no upstream contact from default installed app at runtime; optional ChatLocal update checks send only required metadata. No claim workflow exists until Codex implements and tests it.
