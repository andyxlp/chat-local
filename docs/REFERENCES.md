# References and factual baseline

Research checked 2026-11-26. These sources guide design, not a certification of implementation. Revalidate versions and provider documentation in Phase 0 and before release; provider interfaces evolve.

## Original code and inspiration
- https://github.com/mrkaylabs/easy-mcp/tree/2c94bed8be2594b0759e9bee04b62b205c564fe4 — pinned original reference; repository fork baseline verified at that commit. server.js has 18 tool definitions, OAuth endpoints, tunnel controls, dashboard. The OAuth authorize route issues codes without local owner approval; dashboard reports online unconditionally; Cloudflare exit handler leaves publicUrl populated after a previously established tunnel; shell uses OS-user process execution, not sandbox. Static findings only, not live exploitation or proof of original-author intent.
- https://github.com/decolua/9router — owner-provided inspiration for low-overhead/background UX only; no claim ChatLocal should use its proxy/provider mechanisms. Do not execute its code simply for inspiration.

## Standards and provider docs
- https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2025-11-25/basic/authorization.mdx — versioned MCP authorization, metadata, identity and registration.
- https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2026-07-28/basic/authorization/index.mdx — versioned authorization update; verify current supported revisions and identity metadata/DCR behavior.
- https://github.com/modelcontextprotocol/modelcontextprotocol/blob/main/docs/specification/2026-07-28/basic/transports/streamable-http.mdx — versioned transport revision; request-scoped streams and removal of protocol-level sessions/GET stream. Prior revisions differ, so test actual clients.
- https://modelcontextprotocol.io/ — canonical documentation entry; verify current SDK and stable revisions.
- https://www.rfc-editor.org/rfc/rfc9700 — OAuth 2.0 security best current practice.
- https://www.rfc-editor.org/rfc/rfc7636 — PKCE.
- https://www.rfc-editor.org/rfc/rfc8707 — OAuth resource indicators.
- https://www.rfc-editor.org/rfc/rfc9728 — OAuth protected resource metadata.
- https://www.rfc-editor.org/rfc/rfc8414 — authorization server metadata.
- https://www.rfc-editor.org/rfc/rfc7009 — token revocation.

## Connectivity
- https://tailscale.com/docs/features/tailscale-funnel — public Funnel, encrypted relay (relay does not decrypt contents), device-side TLS termination, beta status, macOS CLI variant prerequisite, bandwidth/port limitations. Stable hostname depends on user retaining provider/device configuration.
- https://developers.cloudflare.com/tunnel/get-started/quick-tunnels/ — temporary URLs, no uptime guarantee, no SSE support, testing/dev only. Use named tunnel for stable service.
- https://developers.cloudflare.com/tunnel/ — official named tunnel setup; verify domain/account and TLS implications.

## AI client and execution patterns
- https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp — cloud-side remote connection, OAuth/client identity choices, connector constraints including shared credentials.
- https://developers.openai.com/api/docs/guides/agents-api/environments/self-hosted — documented outbound executor architecture (design inspiration, not a protocol for universal ChatLocal integration).
- https://developers.openai.com/api/docs/guides/agents-api/environments/security — separation of execution, secrets and network/approval policy.
- https://developers.openai.com/api/docs/guides/agents-api/sessions/approvals — approval patterns, not evidence of consumer ChatGPT rendering ChatLocal approval prompts.
- https://developers.openai.com/ — find current ChatGPT custom MCP/app connection documentation during Phase 0.
- https://ai.google.dev/ — find current Gemini official remote MCP/API capabilities and distinguish consumer web, API and CLI during Phase 0.

Actual ChatGPT, Claude and Gemini interoperability remains unverified. Do not substitute generic agent API documentation for proof that a specific consumer connector works. Do not copy private protocol implementations, claim access to internal OpenAI/Google/Anthropic architecture, or infer universal feature parity.
