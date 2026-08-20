# blackbox-toolkit
Claude Code skills for black-box QA against a live URL: recon, module mapping, and test case generation without access to source or the running project.

## Skills

- **bb-recon** — navigates a live URL with Playwright MCP, fingerprints the tech stack from network requests/response headers, maps visible modules and nav structure, and infers business purpose and high-risk areas. Saves a snapshot to `./bb-recon.md` at the project root. Inferred claims are labeled `[inferred]`, never presented as confirmed, since there's no source to verify against.
- **bb-test-cases** *(planned)* — generates black-box test cases from `bb-recon`'s module map and risk areas.
- **bb-explore** *(planned)* — runs an exploratory testing session against the live URL, guided by `bb-recon`'s output.

## Install

From GitHub, in any project:

```bash
claude plugin marketplace add rcadia/blackbox-toolkit
claude plugin install blackbox-toolkit@blackbox-toolkit-marketplace
```

From a local checkout, for iterating on the skills themselves — edits show up on the next session restart, no push needed:

```bash
claude plugin marketplace add /path/to/blackbox-toolkit
claude plugin install blackbox-toolkit@blackbox-toolkit-marketplace
```

The plugin bundles the Playwright MCP server (via `mcpServers` in `plugin.json`) — no separate MCP setup needed.

## Use

```bash
/bb-recon <url>
```

Navigates the URL, saves a snapshot to `./bb-recon.md` at the project root. Re-running overwrites it — one file, one target at a time.

## Update

After pulling or pushing changes to an installed plugin:

```bash
claude plugin update blackbox-toolkit@blackbox-toolkit-marketplace
```

Then restart the Claude Code session in that project — skills load at session start.
