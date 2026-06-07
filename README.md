<div align="center">

# github-reader-skill

**A cross-platform AI agent skill for reading GitHub file content**

<br>

<!-- Language Switch -->
<a href="#english-version">🇬🇧 English</a> &nbsp;·&nbsp; <a href="#chinese-version">🇨🇳 中文</a>

<br>

![Version](https://img.shields.io/badge/version-1.1.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platforms](https://img.shields.io/badge/platforms-Claude%20Code%20·%20Codex%20·%20Gemini%20CLI%20·%20OpenCode%20·%20Cursor%20·%20Windsurf%20·%20Cline-purple)

</div>

---

<h2 id="english-version">🇬🇧 English</h2>

<div align="right"><a href="#chinese-version">切換中文 →</a></div>

### What it does

Whenever you paste a GitHub URL into your AI agent, this skill automatically fetches the file or directory content directly from GitHub — before answering your question.

Supports three URL patterns:

| URL type | Converts to |
|---|---|
| `github.com/{owner}/{repo}/blob/{branch}/{file}` | `raw.githubusercontent.com/...` (raw file content) |
| `github.com/{owner}/{repo}/tree/{branch}/{dir}` | `api.github.com/repos/.../contents/...` (directory listing) |
| `github.com/{owner}/{repo}` | README.md from `raw.githubusercontent.com` |

**Strict source rule:** Content is only accepted from the exact `raw.githubusercontent.com/{owner}/{repo}/...` path matching the original link. No silent fallbacks to mirrors, forks, or third-party sites.

### Platform compatibility

| Agent | Support | Install path |
|---|---|---|
| Claude Code | ✅ Native | `~/.claude/skills/` |
| Codex CLI | ✅ Native | `~/.agents/skills/` |
| Gemini CLI | ✅ Native | `~/.gemini/skills/` |
| OpenCode | ✅ Native | `~/.config/opencode/skills/` |
| Cursor | 🔄 Requires conversion | `.cursor/rules/` |
| Windsurf | 🔄 Requires conversion | `.windsurf/rules/` |
| Cline | 🔄 Requires conversion | `.clinerules/` |

### Installation

**Via agent (recommended)**

In any supported agent, say:
```
Install this skill: https://github.com/lauytgary/github-reader-skill
```

**Via npx**
```bash
npx skills add https://github.com/lauytgary/github-reader-skill
```

**Manual**
```bash
# Claude Code
git clone https://github.com/lauytgary/github-reader-skill.git
cp -r github-reader-skill/github-reader ~/.claude/skills/

# Codex CLI
cp -r github-reader-skill/github-reader ~/.agents/skills/

# Gemini CLI
cp -r github-reader-skill/github-reader ~/.gemini/skills/
```

### China Mainland note

`raw.githubusercontent.com` is intermittently blocked by the GFW. This skill will:
1. Always try the original URL first
2. If it fails (timeout / connection reset), **ask your permission** before switching to `gh-proxy.com`
3. Only use the proxy after you explicitly approve
4. If the proxy also fails, suggest using a VPN or local `git clone`

### Author

Made by **Gary Lau** · MIT License

---

<h2 id="chinese-version">🇨🇳 中文</h2>

<div align="right"><a href="#english-version">Switch to English →</a></div>

### 功能說明

每當你在 AI Agent 中貼上 GitHub 連結，這個 skill 會在回答你的問題之前，自動從 GitHub 直接抓取檔案或目錄內容。

支援三種 URL 類型：

| URL 類型 | 轉換目標 |
|---|---|
| `github.com/{owner}/{repo}/blob/{branch}/{file}` | `raw.githubusercontent.com/...`（原始檔案內容）|
| `github.com/{owner}/{repo}/tree/{branch}/{dir}` | `api.github.com/repos/.../contents/...`（目錄列表）|
| `github.com/{owner}/{repo}` | 從 `raw.githubusercontent.com` 抓取 README.md |

**嚴格來源規則：** 只接受來自完全匹配原始連結的 `raw.githubusercontent.com/{owner}/{repo}/...` 的內容，不會靜默 fallback 到鏡像站、fork 或第三方網站。

### 平台兼容性

| Agent | 支援方式 | 安裝路徑 |
|---|---|---|
| Claude Code | ✅ 原生支援 | `~/.claude/skills/` |
| Codex CLI | ✅ 原生支援 | `~/.agents/skills/` |
| Gemini CLI | ✅ 原生支援 | `~/.gemini/skills/` |
| OpenCode | ✅ 原生支援 | `~/.config/opencode/skills/` |
| Cursor | 🔄 需要轉換 | `.cursor/rules/` |
| Windsurf | 🔄 需要轉換 | `.windsurf/rules/` |
| Cline | 🔄 需要轉換 | `.clinerules/` |

### 安裝方式

**透過 Agent 安裝（推薦）**

在任何支援的 Agent 中直接說：
```
安裝這個 skill：https://github.com/lauytgary/github-reader-skill
```

**透過 npx 安裝**
```bash
npx skills add https://github.com/lauytgary/github-reader-skill
```

**手動安裝**
```bash
# Claude Code
git clone https://github.com/lauytgary/github-reader-skill.git
cp -r github-reader-skill/github-reader ~/.claude/skills/

# Codex CLI
cp -r github-reader-skill/github-reader ~/.agents/skills/

# Gemini CLI
cp -r github-reader-skill/github-reader ~/.gemini/skills/
```

### 中國大陸用戶說明

`raw.githubusercontent.com` 在中國大陸受 GFW 間歇性封鎖。此 skill 的處理邏輯：

1. 永遠先嘗試原始 URL
2. 若失敗（逾時 / 連線重置），**先詢問你的批準**，再考慮切換到 `gh-proxy.com`
3. 只有在你明確同意後才使用 proxy
4. 若 proxy 也失敗，建議使用 VPN 或本地 `git clone`

### 作者

由 **Gary Lau** 製作 · MIT 授權

