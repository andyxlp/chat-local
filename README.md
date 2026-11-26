# ChatLocal

**Your AI. Your computer. Your control.**

ChatLocal is a planned lightweight, independently maintained macOS bridge that lets approved cloud AI clients use your local workspace through MCP: read, edit, search, create files, run local commands when authorized, and manage jobs.

**Status: engineering handoff, not an installable application.** No ChatLocal executable, release, live connector, or security certification is claimed. Do not run the original Easy MCP reference code or deploy it publicly. Its known OAuth authorization flaw remains in that historical snapshot.

## Start here (Codex or any implementation agent)

1. Read [AGENTS.md](AGENTS.md) and [Product specification](docs/PRODUCT_SPEC.md).
2. Read [Architecture](docs/ARCHITECTURE.md), [Security](SECURITY.md), and [MCP interoperability](docs/MCP_INTEROP.md) before writing network-facing code.
3. Follow [Feature parity](docs/FEATURE_PARITY.md), [Implementation plan](docs/IMPLEMENTATION_PLAN.md), and [Acceptance criteria](docs/ACCEPTANCE_CRITERIA.md).
4. Use [UX and operations](docs/UX_OPERATIONS.md), [Reliability](docs/RELIABILITY.md), [Test plan](docs/TEST_PLAN.md), and [Upstream maintenance](docs/UPSTREAM_SYNC.md).
5. Record implementation decisions and measured evidence in [Delivery status](docs/DELIVERY_STATUS.md). See [Sources and baseline](docs/REFERENCES.md).

The owner wants ChatGPT, Claude and Gemini cloud clients to access authorized local files from anywhere, with optional native command execution, a stable endpoint, menu-bar controls, straightforward setup, low idle resource use, explicit local approval and no hidden developer access. Docker/Electron are not required. No proprietary protocol or AI subscription bypass.

## Provenance

Forked from mrkaylabs/easy-mcp, MIT licensed. Original source at commit 2c94bed8be2594b0759e9bee04b62b205c564fe4 is preserved verbatim as inert text under [reference/easy-mcp-v2.3.1](reference/easy-mcp-v2.3.1/README.md) and in Git history. License and attribution are retained. ChatLocal has separate application identity and storage; the GitHub repository slug may still be easy-mcp until the owner renames it in Settings.

No personal data, tokens, endpoints, local file paths or credentials belong in this public repository.
