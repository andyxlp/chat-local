# Easy MCP

**Your web AI, working on your files. Nothing else.**

Connect a local project folder to Claude Web, ChatGPT or Gemini over the Model Context Protocol. One command and you're in — one web dashboard, lightweight and fast.

▶️ **Watch the demos:**
- [Free Claude & ChatGPT vs Gemini Pro — who controls my PC?](https://www.youtube.com/watch?v=P-xTnxFNU80)
- [I Built a Mini Amazon with Only Gemini Web (Full Stack)](https://youtu.be/9vPKYf_BFlo)

- **Official custom MCP only.** Easy MCP uses the built-in custom MCP connector of your AI client — the standard, first-party way to connect tools. No wrapper, no proxy, no browser automation, no cookies, no login scraping. Nothing here violates any terms of service.
- **One link, no login.** Paste the link into your AI account and you are done. Easy MCP never asks for your credentials and cannot take anything from your account.
- **Everything runs on your machine.** The server is local; the tunnel only carries MCP calls. Nobody can touch your files unless you hand them the link — the link is the key.
- **Nothing executes without your grant.** Folder jail on, shell off by default. Models reason and search on their own cloud VMs; Easy MCP just syncs files. Shell exists for one thing: running your tests when you say so.

If you want shell access available around the clock, run Easy MCP inside a VM, WSL distro or VPS dedicated to it — that is the environment it is happiest in.

## Tunnel modes

Easy MCP publishes your local server through a tunnel. Two modes, switchable
any time from the dashboard toggle:

- **Cloudflare (default)** — a free quick tunnel, no account needed. The URL
  changes on every run, so you re-paste the MCP link after each restart. Note:
  some AI providers' servers get blocked by Cloudflare's bot protection.
- **Tailscale Funnel (persistent link)** — a stable
  `https://<machine>.<tailnet>.ts.net` address that never changes: paste the
  MCP link once and forget it. Requires [Tailscale](https://tailscale.com/download)
  installed and signed in (free plan works).

Your choice is remembered in `~/.easy-mcp/config.json`. Your access token is
generated once and stored in `~/.easy-mcp/token`, so with Funnel the MCP link
stays identical across restarts. Run with `--rotate-token` to mint a new one.

## Quickstart

One command — paste into cmd, PowerShell, or terminal (needs Node.js 18+):

```
npx -y https://codeload.github.com/mrkaylabs/easy-mcp/tar.gz/refs/heads/main
```

That runs Easy MCP on the current folder and opens the dashboard in your
browser. To use another folder: `npx -y https://codeload.github.com/mrkaylabs/easy-mcp/tar.gz/refs/heads/main /path/to/your/project`

Or clone it:

```
git clone https://github.com/mrkaylabs/easy-mcp.git
cd easy-mcp
node easy-mcp.js /path/to/your/project
```

On Windows you can also double-click `easy-mcp.bat`.

### Updating

`npx` caches the downloaded copy, so an installed Easy MCP never updates itself. The launcher checks GitHub on startup and prints a notice when a newer build exists. To update, delete the npx cache folder, then run the same command again:

- Windows: `%LocalAppData%\npm-cache\_npx`
- macOS / Linux: `~/.npm/_npx`

If you cloned the repo instead, `git pull`.

What happens:

1. A local MCP server starts on `127.0.0.1:9494`.
2. The selected tunnel publishes it at a public HTTPS address.
3. Your browser opens the dashboard.

## Dashboard

The dashboard is the control panel:

- **MCP link** — the public link to paste into your web AI client's MCP / custom tool settings, with a Copy button.
- **Project folder** — switch the folder the AI can access, with recent folders one click away.
- **Security toggles** — Read-Only mode, Shell on/off (off by default), folder jail.
- **Status** — tool count, calls, errors, sessions, background jobs, uptime.
- **Recent tool calls** — what the AI did, live.
- **Background jobs** — stop dev servers the AI started.

## Tools the AI gets

`read_file`, `read_files`, `write_file`, `write_files`, `edit_file`, `list_directory`, `search_files`, `get_file_info`, `execute_command`, `start_job`, `job_status`, `stop_job`, `list_jobs`, `git_status`, `git_diff`, `fetch_url`, `get_current_project`, `set_project`.

All file access is jailed to the project folder. Shell tools (`execute_command`, `start_job`) only work while Shell is enabled in the dashboard.

## Options

```
node easy-mcp.js [folder] [--port 9494] [--no-tunnel] [--no-open]
                          [--read-only] [--enable-shell] [--token <token>]
```

- `--read-only` — block file writes.
- `--enable-shell` — start with shell enabled (it is off by default; `--disable-shell` is accepted for compatibility).
- `--dangerously-skip-jail` — **no folder jail**: the AI can read, write and run commands anywhere on the machine (defaults the project to the drive root). Anyone with the MCP link + token gets the same access. The dashboard shows a red warning while it is on.
- `--no-tunnel` — local only, no public link.
- `--no-open` — don't open the browser.

## Security model

- Every request needs the token (`Authorization: Bearer <token>` or `?token=<token>`); a fresh random token is generated per run unless `--token` / `EASY_MCP_TOKEN` is set.
- The server binds `127.0.0.1` only; remote traffic goes through the tunnel.
- File operations cannot escape the project folder (path traversal and absolute paths outside it are rejected, symlinks resolved).
- Shell is off by default and only runs commands inside the project folder when you enable it.
- `fetch_url` only fetches public https addresses: private/loopback hosts are blocked and redirects are capped.
- File reads are size-capped so a stray log or dump file cannot exhaust memory.
- The MCP link is the key: anyone holding it has the same access as the AI. With Cloudflare quick tunnels the link is random per run; with Tailscale Funnel, keep the hostname private.

## Connecting clients

Claude Web, ChatGPT, Gemini Web, Gemini API (Interactions API), Gemini CLI, Antigravity. The `/mcp` endpoint speaks Streamable HTTP (with sessions) plus the legacy SSE transport.

## License

MIT — see [LICENSE](LICENSE).

---

[![Share AI Slot — family slots & AI subscriptions from $1/month](https://raw.githubusercontent.com/mrkaylabs/share-ai-slot/main/banner.png)](https://mrkaylabs.github.io/share-ai-slot/)

**Share AI Slot** — need a Google family slot or an AI subscription (ChatGPT, Claude Code...)? Got a spare one to sell? [Submit here](https://mrkaylabs.github.io/share-ai-slot/) — from $1/month, varies by country.
