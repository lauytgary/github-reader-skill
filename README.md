<div align="center">

# github-reader-skill

**A cross-platform AI agent skill for reading GitHub file content**

<br>

<!-- Language Switch -->
<a href="#english-version">English</a> &nbsp;·&nbsp; <a href="#chinese-version">中文</a>

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

### 功能说明

每当你在 AI Agent 中贴上 GitHub 连结，这个 skill 会在回答你的问题之前，自动从 GitHub 直接抓取档案或目录内容。

支援三种 URL 类型：：

| URL 类型 | 转换目标 |
|---|---|
| `github.com/{owner}/{repo}/blob/{branch}/{file}` | `raw.githubusercontent.com/...`（原始档案内容）|
| `github.com/{owner}/{repo}/tree/{branch}/{dir}` | `api.github.com/repos/.../contents/...`（目录列表）|
| `github.com/{owner}/{repo}` | 從 `raw.githubusercontent.com` 抓取 README.md |

**严格来源规则：** 只接受来自完全匹配原始连结的 `raw.githubusercontent.com/{owner}/{repo}/...` 的内容，不会静默 fallback 到镜像站、fork 或第三方网站。

### 平台兼容性

| Agent | 支援方式 | 安装路径 |
|---|---|---|
| Claude Code | ✅ 原生支援 | `~/.claude/skills/` |
| Codex CLI | ✅ 原生支援 | `~/.agents/skills/` |
| Gemini CLI | ✅ 原生支援 | `~/.gemini/skills/` |
| OpenCode | ✅ 原生支援 | `~/.config/opencode/skills/` |
| Cursor | 🔄 需要转换 | `.cursor/rules/` |
| Windsurf | 🔄 需要转换 | `.windsurf/rules/` |
| Cline | 🔄 需要转换 | `.clinerules/` |

### 安装方式

**透过 Agent 安装（推荐）**

在任何支援的 Agent 中直接说：
```
安装这个 skill：https://github.com/lauytgary/github-reader-skill
```

**透过 npx 安装**
```bash
npx skills add https://github.com/lauytgary/github-reader-skill
```

**手动安装**
```bash
# Claude Code
git clone https://github.com/lauytgary/github-reader-skill.git
cp -r github-reader-skill/github-reader ~/.claude/skills/

# Codex CLI
cp -r github-reader-skill/github-reader ~/.agents/skills/

# Gemini CLI
cp -r github-reader-skill/github-reader ~/.gemini/skills/
```

### 中国大陆用户说明

`raw.githubusercontent.com` 在中国大陆受间歇性封锁。此 skill 的处理逻辑：

1. 永远先尝试原始 URL
2. 若失败（逾时 / 连线重置），**先询问你的批准**，再考虑切换到 `gh-proxy.com`
3. 只有在你明确同意后才使用 proxy
4. 若 proxy 也失败，建议使用 VPN 或本地 `git clone`

### 作者

由 **Gary Lau** 制作 · MIT 授权

