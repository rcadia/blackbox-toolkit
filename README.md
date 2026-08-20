# blackbox-toolkit
Claude Code skills for black-box QA against a live URL: recon, module mapping, and test case generation without access to source or the running project.

## Skills

- **bb-recon** — navigates a live URL with Playwright MCP, fingerprints the tech stack from network requests/response headers, maps visible modules and nav structure, and infers business purpose and high-risk areas. Saves a snapshot to `./bb-recon.md` at the project root. Inferred claims are labeled `[inferred]`, never presented as confirmed, since there's no source to verify against.
- **bb-test-cases** *(planned)* — generates black-box test cases from `bb-recon`'s module map and risk areas.
- **bb-explore** *(planned)* — runs an exploratory testing session against the live URL, guided by `bb-recon`'s output.

## Install

Add this repo as a marketplace and install the `blackbox-toolkit` plugin, then run `/bb-recon <url>`. The plugin bundles the Playwright MCP server (via `mcpServers` in `plugin.json`) — no separate MCP setup needed.
