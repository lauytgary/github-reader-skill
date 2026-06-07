---
name: github-reader
metadata:
  author: Gary Lau
  version: 1.1.0
compatibility: "native: claude-code, codex-cli, gemini-cli, opencode | requires-conversion: cursor, windsurf, cline"
description: >
  Automatically fetch and read GitHub file/repo/directory content whenever the user provides a GitHub URL.
  Trigger this skill any time a GitHub link appears in the user's message — even if they don't explicitly
  ask to "read" or "fetch" it. This includes github.com URLs in passing, e.g. "what does this do?
  github.com/owner/repo/blob/main/foo.py". Handle three URL patterns:
  (1) file links (/blob/) → fetch raw file content,
  (2) directory links (/tree/) → list directory contents via GitHub API,
  (3) repo root links (github.com/owner/repo) → fetch the README.
  Always use this skill before answering any question that references a GitHub URL.
---

# GitHub Reader Skill

> **Author:** Gary Lau
> **Version:** 1.1.0

**Native support:** Claude Code · Codex CLI · Gemini CLI · OpenCode
**Requires conversion:** Cursor (→ `.mdc`) · Windsurf (→ `.md` rules) · Cline (→ stripped `.md`)

Whenever the user provides a GitHub URL, convert it to a fetchable form and retrieve the content **before** answering their question.

---

## URL Conversion Rules

### 1. File link (`/blob/`)

```
github.com/{owner}/{repo}/blob/{branch}/{path}
        ↓
raw.githubusercontent.com/{owner}/{repo}/{branch}/{path}
```

**Example:**
```
https://github.com/anthropics/anthropic-sdk-python/blob/main/README.md
→ https://raw.githubusercontent.com/anthropics/anthropic-sdk-python/main/README.md
```

---

### 2. Directory link (`/tree/`)

```
github.com/{owner}/{repo}/tree/{branch}/{path}
        ↓
api.github.com/repos/{owner}/{repo}/contents/{path}?ref={branch}
```

Returns a JSON list of files and subdirectories. Parse and present as a readable file tree.

**Example:**
```
https://github.com/anthropics/anthropic-sdk-python/tree/main/src
→ https://api.github.com/repos/anthropics/anthropic-sdk-python/contents/src?ref=main
```

---

### 3. Repo root (`github.com/{owner}/{repo}`)

No `/blob/`, `/tree/`, `/pull/`, `/issues/` in path — just owner + repo.

Fetch the README at:
```
raw.githubusercontent.com/{owner}/{repo}/main/README.md
```

Fallback order if not found: `main` → `HEAD` → `master`.

---

## Fetching the Converted URL

For most agents, fetch the converted URL directly using your available HTTP tool (`curl`, `requests`, `fetch`, etc.).

**Strict source rule:** Only accept content from `raw.githubusercontent.com/{owner}/{repo}/...`
where owner and repo **exactly match** the user's original link. Do not substitute PyPI, npm,
third-party mirrors, forks, or any other source — even if the content looks identical.
If the content cannot be retrieved from the correct repo, tell the user rather than silently
falling back to another source.

### Claude Code — additional requirement

`web_fetch` in Claude Code only accepts URLs directly provided by the user or that appeared
in prior search results. Converted URLs will be rejected even if correct. Always do a
`web_search` step first:

1. Search: `raw.githubusercontent.com {owner}/{repo} {filename}`
2. Confirm the result URL starts with `https://raw.githubusercontent.com/{owner}/{repo}/`
3. Only then call `web_fetch` on that URL

---

## After Fetching

Briefly note that you fetched directly from GitHub, then answer the user's question:
> *"Fetched the raw file from GitHub — here's what it contains:"*
> *"Fetched the repo README from GitHub:"*

---

## Edge Cases

| Situation | Action |
|---|---|
| URL has no scheme (`github.com/...`) | Prepend `https://` before converting |
| Branch name contains `/` (e.g. `feature/my-branch`) | Preserve as-is in the raw URL path |
| `/blob/` URL has a line anchor (`#L42`) | Strip the anchor before fetching |
| Fetch returns 404 | Tell the user the file wasn't found; suggest checking branch name or path |
| Private repo (401/403) | Inform the user the repo is private and can't be fetched without a token |
| Non-content URLs (`/pull/`, `/issues/`, `/actions/`) | Do NOT apply this skill |
| **China Mainland — fetch fails (timeout / connection reset)** | See section below |

---

## China Mainland Fallback

`raw.githubusercontent.com` and `api.github.com` are intermittently blocked in mainland China via DNS pollution and TCP reset. If a fetch fails with a timeout or connection error (not a 404 or 401):

**Step 1 — Always try the original URL first.**
Attempt the standard `raw.githubusercontent.com` fetch normally.

**Step 2 — On failure, ask before switching.**
If the fetch times out or is reset, do NOT silently retry with a proxy. Instead, tell the user:

> "Failed to reach `raw.githubusercontent.com` — this may be due to blocking.
> I can retry using `gh-proxy.com` as a mirror proxy, but note it is a third-party service.
> Would you like me to try via `gh-proxy.com`?"

**Step 3 — Only on explicit user approval**, retry with:
```
https://gh-proxy.com/https://raw.githubusercontent.com/{owner}/{repo}/{branch}/{path}
```
For API (directory) requests, retry with:
```
https://gh-proxy.com/https://api.github.com/repos/{owner}/{repo}/contents/{path}?ref={branch}
```

**Step 4 — If proxy also fails**, inform the user and suggest using a VPN or cloning the repo locally.
