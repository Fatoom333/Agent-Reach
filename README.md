<h1 align="center">👁️ Agent Reach</h1>

<p align="center">
  <strong>Give your AI Agent one-click access to the internet</strong>
</p>

<p align="center">
  <a href="https://github.com/Panniantong/Agent-Reach">Panniantong/Agent-Reach</a> — kept manual-invoke only, see <a href="#this-fork">below</a>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="MIT License"></a>
  <a href="https://www.python.org/"><img src="https://img.shields.io/badge/Python-3.10+-green.svg?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.10+"></a>
  <a href="https://github.com/Fatoom333/Agent-Reach/releases"><img src="https://img.shields.io/github/v/release/Fatoom333/Agent-Reach?style=for-the-badge" alt="Latest release"></a>
</p>

<p align="center">
  <a href="#quick-start">Quick Start</a> · <a href="#this-fork">This Fork</a> · <a href="#supported-platforms">Platforms</a> · <a href="#design-philosophy">Design</a> · <a href="docs/README_ja.md">日本語*</a> · <a href="docs/README_ko.md">한국어*</a>
</p>

<p align="center"><sub>*Translations of the upstream project. They have not been updated for this fork's changes — see <a href="#this-fork">This Fork</a> for what's different here.</sub></p>

---

## This fork

This is a personal fork of [Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach). The upstream project's skill description told agents to "MUST USE" it on any link, platform name, or research request — so it got loaded automatically on almost every turn. This fork changes that, and nothing else about the underlying tool.

| What | Upstream behavior | This fork |
|------|-------------------|-----------|
| Skill activation | Loads itself on any URL, platform name, or search/research intent | Loads **only** on `/agent-reach` or a direct "use agent-reach" request |
| Network calls | Standing rule told the agent to run `agent-reach check-update` on its own after a task | No autonomous network calls — version checks are your call |
| `agent-reach skill --install` | Writes to every agent-client directory that happens to exist (`~/.agents`, `~/.config/opencode`, `~/.openclaw`, `~/.claude`) | Writes to `~/.claude/skills/` only; other clients need an explicit `--skill-dir DIR` or `--all-clients` |
| `agent-reach skill --uninstall` | Removes from every directory it finds | Same default scope as install; copies left in other clients are reported, not deleted silently |
| Slash command | None | [`.claude/commands/agent-reach.md`](.claude/commands/agent-reach.md) — explicitly loads the skill and runs your request through it |
| Update cycle | Points at upstream, which would reinstall the auto-triggering skill | Install/update docs, the CLI's `check-update`/`watch` output, and the MCP install hint all point at this fork |

Everything else — the channel routing, the upstream tools it calls, the CLI itself — is unmodified from upstream. Full detail is in [CHANGELOG.md](CHANGELOG.md) (the `1.6.0` entry).

If you just want the original, always-on-topic skill, use [the upstream project](https://github.com/Panniantong/Agent-Reach) instead. If you want a skill that only runs when you explicitly ask for it, you're in the right place.

---

## Why Agent Reach?

AI Agents can already write code, edit docs, and manage projects — but ask one to go look something up online and it stalls:

- 📺 "Summarize this YouTube video" → **can't**, no way to grab subtitles
- 🐦 "What are people saying about this on Twitter?" → **can't**, the Twitter API is paid
- 📖 "Check Reddit for this bug" → **403**, server IPs get blocked
- 📕 "What's the buzz on XiaoHongShu about this product?" → **can't open it**, login required
- 📺 "Summarize this Bilibili video" → **can't fetch it**, generic downloaders are blocked
- 🔍 "Search the web for the latest LLM framework comparisons" → **no good search**, paid or low quality
- 🌐 "What's on this page?" → **a wall of raw HTML**, unreadable
- 📦 "What does this GitHub repo do? What's in the issues?" → works, but auth setup is a chore
- 📡 "Watch these RSS feeds for updates" → have to install a library and write code

**None of this is hard — it's just tedious to set up.** Every platform has its own hurdle: a paid API, a block to work around, a login to manage, messy output to clean up. You end up hunting down tools, installing dependencies, and debugging configs one platform at a time.

**Agent Reach turns that into one message:**

```
Install Agent Reach: https://raw.githubusercontent.com/Fatoom333/Agent-Reach/main/docs/install.md
```

Paste that to your Agent, and within a few minutes it can read tweets, search Reddit, watch YouTube, and browse XiaoHongShu.

**Already installed? Updating is one message too:**

```
Update Agent Reach: https://raw.githubusercontent.com/Fatoom333/Agent-Reach/main/docs/update.md
```

### ✅ Before you install, you might want to know

| | |
|---|---|
| 💰 **Free** | Every tool is open source and every API is free. The only thing that might cost money is a server proxy ($1/month) — local machines don't need one |
| 🔒 **Private** | Cookies stay on your machine, never uploaded. Code is fully open source and auditable |
| 🔄 **Kept current** | Every platform routes through a primary + fallback backend list. When one breaks, it switches to the next without you noticing |
| 🤖 **Works with any Agent** | Claude Code, OpenClaw, Cursor, Windsurf — anything that can run shell commands |
| 🩺 **Self-diagnosing** | `agent-reach doctor` — one command tells you what's working, what isn't, and how to fix it |

---

## Supported platforms

| Platform | Works out of the box | Unlocks with setup | How to set up |
|----------|----------------------|---------------------|----------------|
| 🌐 **Web** | Read any page | — | No setup |
| 📺 **YouTube** | Subtitles + video search | — | No setup |
| 📡 **RSS** | Read any RSS/Atom feed | — | No setup |
| 🔍 **Web search** | — | Semantic search across the whole web | Auto-configured (MCP, free, no key) |
| 📦 **GitHub** | Read public repos + search | Private repos, Issues/PRs, forks | Tell your Agent "help me log into GitHub" |
| 🐦 **Twitter/X** | Read a single tweet | Search, timeline, long-form posts | Tell your Agent "help me set up Twitter" |
| 📺 **Bilibili** | Search + video detail (bili-cli, no login) | Subtitles (via OpenCLI) | Tell your Agent "help me set up Bilibili" |
| 📖 **Reddit** | — (no zero-config path; anonymous endpoints are blocked) | Search, read posts and comments | Desktop: OpenCLI via your browser session; or rdt-cli + cookie |
| 📘 **Facebook** | — | Search, profile, feed, groups list | Desktop: OpenCLI (reuses your Chrome session) |
| 📷 **Instagram** | — | User search, profile, recent posts | Desktop: OpenCLI (reuses your Chrome session) |
| 📕 **XiaoHongShu** | — | Search, read, comments | OpenCLI uses only an existing Chrome session you already control; MCP/legacy tools use a manual Cookie-Editor export |
| 💼 **LinkedIn** | Jina Reader for public pages | Full profiles, company pages, job search | Tell your Agent "help me set up LinkedIn" |
| 💻 **V2EX** | Hot topics, node topics, topic detail + replies, user profile | — | No setup |
| 📈 **Xueqiu (stocks)** | Quotes, search, hot posts, hot stocks | — | Tell your Agent "help me set up Xueqiu" |
| 🎙️ **Xiaoyuzhou podcasts** | — | Audio-to-text transcription (Whisper, free key) | Tell your Agent "help me set up Xiaoyuzhou" |

> **Not sure how to set something up? You don't have to read the docs.** Just tell your Agent "help me set up X" and it will walk you through it.
>
> 🍪 Twitter only accepts a value you export by hand via Cookie-Editor. Agent Reach never logs a user into XiaoHongShu and never reads its browser cookies; OpenCLI only uses a Chrome session you already have and already control. `agent-reach configure xhs-cookies` does not inject cookies into OpenCLI or Chrome — without an existing session, export manually via Cookie-Editor and configure xiaohongshu-mcp or a legacy tool instead.
>
> Saved Twitter cookies are only used by `agent-reach doctor` to check that credentials are present. Running the upstream `twitter` command directly still requires `TWITTER_AUTH_TOKEN` and `TWITTER_CT0` set explicitly in that process's environment.
>
> 🔒 Cookies never leave your machine. Code is fully open source and auditable.
> 💻 Local machines don't need a proxy. A proxy is only needed when running on a server (~$1/month).

---

## Quick start

> ⚠️ **OpenClaw users: enable exec permission first**
>
> Agent Reach needs the Agent to run shell commands (`pip install`, `mcporter`, `twitter`, etc.). If your OpenClaw uses the default `messaging` tool profile, the Agent won't be able to run them. **Enable exec before installing**:
>
> ```bash
> openclaw config set tools.profile "coding"
> ```
> Or set `"tools": { "profile": "coding" }` in `~/.openclaw/openclaw.json`, then restart the Gateway (`openclaw gateway restart`) and start a new conversation. Other platforms (Claude Code, Cursor, Windsurf, etc.) aren't affected.

Paste this to your AI Agent (Claude Code, OpenClaw, Cursor, etc.):

```
Install Agent Reach: https://raw.githubusercontent.com/Fatoom333/Agent-Reach/main/docs/install.md
```

That's it — the Agent handles the rest.

> 🔄 **Already installed?** Updating is one message too:
> ```
> Update Agent Reach: https://raw.githubusercontent.com/Fatoom333/Agent-Reach/main/docs/update.md
> ```

> 🛡️ **Safe by default:** `agent-reach install` only checks your environment by default — it never installs system packages or writes config on its own:
> ```
> Safely check and install Agent Reach: https://raw.githubusercontent.com/Fatoom333/Agent-Reach/main/docs/install.md
> ```
> Only use `agent-reach install --system` once you've explicitly approved changes to your machine.

<details>
<summary>What it actually does (click to expand)</summary>

1. **Installs the CLI** — the `agent-reach` command from this repo (bundles yt-dlp, feedparser; don't install the identically-named PyPI package, it's a different project)
2. **Checks system dependencies** — Node.js, gh CLI, mcporter, and tells you how to get anything missing
3. **Installs/configures only with your approval** — dependencies and Exa via MCP are installed only when you explicitly pass `--system`
4. **Detects your environment** — local machine vs. server, with matching setup advice
5. **Registers the skill only with your approval** — writes the Agent's skill directory only when you pass `--system`; the default check never touches files
6. **Asks before unlocking more** — only the 6 zero-config channels activate by default; anything needing a login (XiaoHongShu, Twitter, Reddit, Facebook, Instagram) is offered as a menu, and only installed if you ask for it by name

Once installed, `agent-reach doctor` is one command that shows the status of every channel and which backend it's currently using.
</details>

---

## Manual invocation only

The skill in this fork **is never selected automatically**. It only runs when you explicitly name it:

```
/agent-reach look up X for me
```

or say directly "use agent-reach to look up X".

- The `description` in `SKILL.md` / `SKILL_en.md` explicitly forbids self-activation: a link, a platform name (Twitter, Bilibili, Reddit, YouTube, GitHub, XiaoHongShu…), a search or research request, or even something noticed while reading a file — none of that should make the Agent reach for this skill on its own.
- The standing rule that told the skill to run `agent-reach check-update` on its own after finishing a task has been removed. The skill makes no autonomous network calls; check for updates yourself with `agent-reach check-update` whenever you want.
- The slash command lives at [`.claude/commands/agent-reach.md`](.claude/commands/agent-reach.md) in this repo. To use it in every project, copy it to `~/.claude/commands/agent-reach.md`.
- The skill installs to `~/.claude/skills/` only by default. To install it for other agent clients, say so explicitly:

```bash
agent-reach skill --install                       # writes ~/.claude/skills/ only
agent-reach skill --install --skill-dir ~/.agents/skills
agent-reach skill --install --all-clients          # every known client directory
agent-reach skill --uninstall                      # same default scope: ~/.claude/skills/ only
```

---

## Works out of the box

No setup needed — just tell your Agent:

- "Take a look at this link" → `curl https://r.jina.ai/URL` reads any web page
- "What is this GitHub repo?" → `gh repo view owner/repo`
- "What's in this YouTube video?" → `yt-dlp` pulls subtitles
- "Search Bilibili for AI tutorials" → `bili search` (no login needed)
- "Search the web for LLM framework comparisons" → Exa semantic search
- "Subscribe to this RSS feed" → parsed with `feedparser`

**No commands to memorize.** The Agent reads the skill file (once you invoke it) and knows what to call. Channels that need a login (XiaoHongShu, Twitter, Reddit, Facebook, Instagram) unlock the moment you say "help me set up X".

---

## Design philosophy

**Agent Reach is a capability layer, not another tool.**

It sits one level above any specific implementation — it handles **selection, installation, health checks, and routing**, not the reading itself. Reading is done by the Agent calling upstream tools directly; there's no wrapper layer in between.

Setting up a new Agent always means the same chores: what reads Twitter? How do you log into Reddit? What replaces a XiaoHongShu CLI that stopped being maintained? Agent Reach does one simple thing: **it picks, installs, and health-checks the most reliable access path for each platform, so you don't relearn this every time an access path changes underneath you.**

### 🔌 Every platform = an ordered list of primary + fallback backends

Switching access paths means reordering the list, not rewriting code. `agent-reach doctor` tells you **which backend each platform is using right now**.

```
channels/
├── web.py          → Jina Reader
├── twitter.py      → twitter-cli ▸ OpenCLI ▸ bird
├── youtube.py      → yt-dlp
├── github.py       → gh CLI
├── bilibili.py     → bili-cli ▸ OpenCLI ▸ search API (yt-dlp is blocked here, retired)
├── reddit.py       → OpenCLI ▸ rdt-cli (no zero-config path, login required)
├── facebook.py     → OpenCLI (desktop browser session)
├── instagram.py    → OpenCLI (desktop browser session)
├── xiaohongshu.py  → OpenCLI ▸ xiaohongshu-mcp ▸ xhs-cli
├── linkedin.py     → mcp-server-linkedin ▸ Jina Reader
├── rss.py          → feedparser
├── exa_search.py   → Exa via mcporter
└── __init__.py     → channel registry (used by doctor)
```

Each channel file **actually probes** its candidate backends in order (not just checking that a command exists); the first one that's fully working is used, and a broken one comes with a fix. The actual reading and searching is done by the Agent calling the upstream tools directly.

### Current backend choices

| Scenario | Primary | Fallback | Why |
|----------|---------|----------|-----|
| Read a web page | [Jina Reader](https://github.com/jina-ai/reader) | — | Free, no API key |
| Read tweets | [twitter-cli](https://github.com/public-clis/twitter-cli) | [OpenCLI](https://github.com/jackwener/opencli) | Stable search in practice; OpenCLI falls back on your browser session |
| Reddit | [OpenCLI](https://github.com/jackwener/opencli) (desktop) | [rdt-cli](https://github.com/public-clis/rdt-cli) | Anonymous endpoints are blocked and the official API is gated — a logged-in session is the only route left |
| Facebook | [OpenCLI](https://github.com/jackwener/opencli) (desktop) | — | Graph/Groups API access is heavily restricted; a browser session is the practical route right now |
| Instagram | [OpenCLI](https://github.com/jackwener/opencli) (desktop) | Official Graph API (Business/Creator + review) | instaloader-style approaches are unstable; OpenCLI reuses a real browser session |
| YouTube subtitles + search | [yt-dlp](https://github.com/yt-dlp/yt-dlp) | — | 154K stars, still the best fit for YouTube (no longer used for Bilibili) |
| Bilibili | [bili-cli](https://github.com/public-clis/bilibili-cli) | OpenCLI ▸ search API | yt-dlp is blocked by Bilibili's anti-scraping; bili-cli searches and reads without login |
| Search the web | [Exa](https://exa.ai) via [mcporter](https://github.com/nicobailon/mcporter) | — | AI semantic search, MCP integration, no key required |
| GitHub | [gh CLI](https://cli.github.com) | — | Official tool, full API access after auth |
| Read RSS | [feedparser](https://github.com/kurtmckee/feedparser) | — | Standard choice in the Python ecosystem |
| XiaoHongShu | [OpenCLI](https://github.com/jackwener/opencli) (desktop) | [xiaohongshu-mcp](https://github.com/xpzouying/xiaohongshu-mcp) (server) ▸ xhs-cli | OpenCLI only uses a session you already have; other backends need a manual Cookie-Editor export |
| LinkedIn | [mcp-server-linkedin](https://github.com/stickerdaniel/linkedin-mcp-server) | Jina Reader | MCP server, browser automation |

> 📌 These are the *current* choices, reviewed regularly against real usage. When a path breaks, it gets swapped for the next one — `agent-reach doctor` always tells you which one is active.

---

## Security

Agent Reach is designed with security in mind:

| Measure | Detail |
|---------|--------|
| 🔒 **Credentials stay local** | Cookies and tokens live only in `~/.agent-reach/config.yaml` on your machine, file permissions `600` (owner read/write only), never uploaded |
| 🛡️ **Safe by default** | `agent-reach install` never modifies your system by default; external tools and config are only written with an explicit `--system` |
| 👀 **Fully open source** | The code is transparent and auditable, and so are all the tools it depends on |
| 🔍 **Dry run** | `agent-reach install --dry-run` previews every action without making changes |
| 🧩 **Pluggable** | Don't trust a component? Swap out its channel file — nothing else is affected |

### 🍪 Cookie safety

> ⚠️ **Ban risk:** platforms accessed via cookie login (Twitter, XiaoHongShu, etc.) can flag script/API-driven access. Use a **dedicated secondary account** for these, not your main one.

Platforms that need cookies or a login (Twitter, XiaoHongShu, Reddit, Facebook, Instagram, etc.) are safest with a **dedicated secondary account**, for two reasons:
1. **Ban risk** — platforms can detect non-browser API-style traffic and restrict or ban the account
2. **Blast radius** — a cookie is equivalent to full login access; using a secondary account limits the damage if it leaks

### 📦 Install modes

| Mode | Command | When to use |
|------|---------|-------------|
| Safe check (default) | `agent-reach install --env=auto` | Any environment; read-only check that lists what's missing |
| Explicit system install | `agent-reach install --env=auto --system` | Once you've explicitly approved changes to this machine |
| Compatibility alias | `agent-reach install --env=auto --safe` | Same as the default behavior |
| Preview only | `agent-reach install --env=auto --dry-run` | See what would happen first |

### 🗑️ Uninstall

```bash
agent-reach uninstall
```

Removes: `~/.agent-reach/` (all tokens/cookies), each Agent's skill files, and the MCP entries in `mcporter`.

```bash
# Preview only, nothing is deleted
agent-reach uninstall --dry-run

# Remove skill files only, keep token config (useful when reinstalling)
agent-reach uninstall --keep-config
```

To remove the Python package itself: `pip uninstall agent-reach`

---

## Credits

Original project: [Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach).

Upstream tools this project routes to: [OpenCLI](https://github.com/jackwener/opencli) · [twitter-cli](https://github.com/public-clis/twitter-cli) · [rdt-cli](https://github.com/public-clis/rdt-cli) · [xiaohongshu-mcp](https://github.com/xpzouying/xiaohongshu-mcp) · [xhs-cli](https://github.com/jackwener/xiaohongshu-cli) · [bili-cli](https://github.com/public-clis/bilibili-cli) · [yt-dlp](https://github.com/yt-dlp/yt-dlp) · [Jina Reader](https://github.com/jina-ai/reader) · [Exa](https://exa.ai) · [mcporter](https://github.com/nicobailon/mcporter) · [feedparser](https://github.com/kurtmckee/feedparser) · [mcp-server-linkedin](https://github.com/stickerdaniel/linkedin-mcp-server)

Issues with the manual-invocation changes in this fork: [Fatoom333/Agent-Reach/issues](https://github.com/Fatoom333/Agent-Reach/issues). Issues with Agent Reach itself (a channel breaking, a new platform request): [the upstream repo](https://github.com/Panniantong/Agent-Reach/issues).

## License

[MIT](LICENSE)
