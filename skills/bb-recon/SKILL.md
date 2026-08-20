---
name: bb-recon
description: Black-box reconnaissance of a live URL with no source or project access. Navigates the site with Playwright MCP, captures network requests and response headers for tech-stack fingerprinting, maps visible modules/nav structure, and produces a snapshot doc with inferred (not confirmed) business purpose and high-risk areas. Saves to ./bb-recon.md at the project root — the only file this skill touches. Use for "recon this site", "fingerprint this URL", "map this app's modules", or as the first step before bb-test-cases/bb-explore against a live URL you don't have source for.
argument-hint: "<url>"
arguments: [url]
disable-model-invocation: true
---

Do black-box reconnaissance against a live URL: **$url**. You have no source, no repo, no server, no logs — report only what a browser and network traffic actually show. Produce a narrated recon briefing in chat, then save it as a snapshot doc — the only file this skill touches. Otherwise read-only: create, edit, or delete nothing else, and never submit forms, attempt logins, or take any state-changing action against the target. Recon is passive observation only.

## Ground rule: observed vs. inferred

Every claim you write is one of two things, and every claim must be labeled which:

- **Observed** — a raw fact you captured directly: a response header value, a status code, a request URL, visible page text, a nav label. State it plainly, no hedging.
- **Inferred** — a conclusion drawn *from* observed facts: "this is Next.js" (from `__NEXT_DATA__` / `x-powered-by`), "this looks like an e-commerce checkout" (from visible copy and URL paths), "this is high risk" (from what a form appears to do). Tag every one `[inferred]` with the observation it rests on. Never state an inferred conclusion as fact — there's no source to verify it against.

Unsure which bucket something belongs in? It's inferred.

## 1. Navigate and capture network traffic

Navigate to $url with the Playwright MCP browser tools (`browser_navigate`). Take a snapshot (`browser_snapshot`) and pull the network log (`browser_network_requests`) for the initial page load. For each notable request, capture method, URL, status code, and response headers — especially `server`, `x-powered-by`, `set-cookie` (names only, never cookie values), `content-security-policy`, cache/CDN headers (`cf-ray`, `x-vercel-id`, `x-amz-cf-id`, etc.), and any API/XHR calls fired on load — these hint at a backend even when you can't see it directly.

List the observed requests and headers plainly. This is raw data, not inference.

## 2. Fingerprint the tech stack `[inferred]`

Read the headers, script/link tags, cookie names, meta generator tags, and global JS objects visible in the snapshot (`__NEXT_DATA__`, `window.__NUXT__`, React DevTools hooks, `wp-content` paths, Shopify/Webflow/Squarespace asset patterns). Infer: frontend framework, backend/CDN/hosting, analytics/marketing tags, third-party services (payment, auth, chat, A/B testing). Tag each conclusion `[inferred]` with the observation it rests on. Thin evidence gets "no signal for backend framework," not a guess.

## 3. Map visible modules and nav structure (observed)

Crawl the top-level nav and any obvious in-app sections, 1–2 links deep, submit nothing. For each: label, URL/path, one-line description of what's on the page. This is observed structure, not inference — it's what's rendered on screen.

## 4. Infer the business purpose `[inferred]`

From visible copy, page titles, meta description, nav labels, and the module map, state in 2–3 sentences what this site/app is for and who it's for. Tag it `[inferred]` — you're reading intent off copy and layout, not a spec.

## 5. Flag high-risk areas `[inferred]`

Flag what would carry more risk in black-box testing: auth/login, payment/checkout, file upload, search, any form collecting PII, third-party integrations/redirects, admin-looking routes, client-side validation with no visible server-side mirror. Name the observed page/element behind each flag and tag the risk judgment `[inferred]` — you can't see the server-side implementation, only what's exposed at the surface.

## Save the snapshot

Save to `./bb-recon.md` at the project root, overwriting any file already there wholesale — this is a point-in-time snapshot, not a hand-maintained doc, and it fully regenerates every run. Head it with the target URL, the generation date, and a note that it's overwritten on the next run. One file, one target: re-running against a different URL overwrites the previous target's snapshot.

Structure the file as:

```
# bb-recon: <url>
Generated <date>. Regenerated wholesale on each run — do not hand-edit.

## Observed: network requests & headers
## Inferred: tech stack
## Observed: modules & nav structure
## Inferred: business purpose
## Inferred: high-risk areas
```

## Definition of done

- Every section above is present, even where the honest content is "no signal found."
- Every claim traces to something actually observed (a header, a request, visible text) — nothing from general knowledge of "what sites like this usually do."
- Every inferred conclusion is tagged `[inferred]` and states what observation it rests on. Nothing inferred is presented as confirmed fact.
- No state-changing action was taken against the target: no form submissions, no login attempts, no writes.
- The snapshot is saved to `./bb-recon.md` at the project root, dated, marked as a regenerated snapshot. No other file was created, edited, or deleted.

## What's next

`bb-test-cases` (test case generation from the module map and risk areas) and `bb-explore` (exploratory testing session guided by the same) consume this snapshot — both planned, not yet built. Tell the user the saved file path when you finish.
