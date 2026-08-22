# DSH CyberWorkStation — 绪山真寻 (Mahiro Oyama) themed beauty rework

[中文](README.zh.md) | **English**

> A 绪山真寻 (Mahiro Oyama, from *OniMai*) themed beauty rework of the DSH Suite workbench by **bailing**, turning [DeepSeek Harness (dsh)](https://github.com/deepseek-ai/deepseek-harness) from a CLI tool into a visual workstation: a Mahiro-themed anime desktop launcher + 9 production-grade plugins + 6 engineering skills + 1 in-dsh skill.
> **Zero core rewrites** — every capability is delivered through official plugin extension points, so the upstream core stays independently upgradable at all times.

![Dark theme](docs/screenshots/onimai-dark.jpg)

![Light theme](docs/screenshots/onimai-light.jpg)

---

## ✦ Beauty highlights

This rework focuses on making every pixel of the launcher feel like the onimai.jp website:

- **绪山真寻 skin** — full launcher reskin in Mahiro's style (shipped as the `onimai` launcher skin): sidebar with the official onimai.jp website's background layout, circular avatar button at the bottom-left, particle background, ZCOOL KuaiLe font, authentic proportions and hover states;
- **Light / dark / follow-system themes** — switchable on the Skins page or by **double-clicking the bottom-left image**; each theme gets its own console-log artwork: dark mode uses a moody background with a dark overlay and red-on-black text shadow for readability, light mode uses a warm cream palette with a lighter overlay;
- **One button, two gestures** — the bottom-left image is both the language switch and the theme switch: **single-click** toggles zh/en with a full-circle sweep animation and reload, **double-click** toggles light/dark instantly without reload;
- **Rebranded skin center** — the dsh-skin-center plugin (Apache-2.0 by [@linxin666](https://github.com/zhu1090093659/dsh-web-ui)) is locally rebranded and preloaded with **16 community skins** (Maid Atelier, Furina, Miku, Matrix, Dragon Heir, Whale Song, …), switchable on the Skins page;
- **Polished details** — full-circle sweep transition, circular buttons with the original site's proportions, floating particles, animated refresh/loading effects, bilingual UI.

## ✦ What is this

DeepSeek Harness is DeepSeek's official agent framework — powerful, but natively CLI-only with a plain web UI. This project adds on top of it:

- **A desktop launcher** (UX inspired by 秋叶 aaaki's ComfyUI packs): double-click an EXE for one-click start/stop, plugin market, skill market, skin market, token analytics, session browser, and one-click updates — all graphical;
- **The Control Deck**: SillyTavern-grade multi-entry leveled prompt injection, regex scripts, World Info lorebooks, and sampling overrides, edited in a GUI and hot-reloaded within 1.5 s;
- **A cost stack**: live multi-provider balances, automatic model price sync, cache-hit rates, and a CC-style usage heatmap;
- **Safety & speed**: destructive-command blocking, hover model prices, HTTP quick-workspace creation, and frontend skin injection.

Everything ships as **plugins / skills / a standalone launcher** — `git status` in the core repo stays clean forever.

## ✦ Quick start

Prerequisites: Windows 10/11, [Git](https://git-scm.com/), [Node.js ^22.19 || >=24](https://nodejs.org/), Edge or Chrome.

```bat
git clone https://github.com/bailingsancun/DSH-CyberWorkStation-skin.git
cd DSH-CyberWorkStation-skin
setup.cmd
```

`setup.cmd` automatically: clones the upstream core **pinned to `dsh-v0.1.0-rc.8`** (the plugin API this suite targets; ~1.5 GB, skipped when a local `core/` already exists) → installs deps and builds (build:lib + build:web) → registers all 9 plugins into the web profile → opens the launcher.

Daily use afterwards: double-click `launcher/DSH启动器.exe`. Manual mode: `node launcher/server.mjs` then visit `http://127.0.0.1:3090`.

> Remember to configure an API key (`OPENROUTER_API_KEY` / `DEEPSEEK_API_KEY` env vars, or `~/.dsh/settings.yaml`).
> Core checked out elsewhere? Point `DSH_REPO` at it; override the launcher port with `DSH_LAUNCHER_PORT`.

## ✦ How it differs from stock dsh (feature matrix)

| Capability | Stock dsh | Suite | Form | Authorship |
|---|---|---|---|---|
| Visual management workbench | ✗ CLI only | 10+ pages, zh/en bilingual, light/dark/system | Standalone app (EXE + zero-dep Node server) | Original |
| One-click start / stop | ✗ manual commands | Auto-opens browser on start; stop reaps the whole console process tree | Launcher | Original |
| Embedded console | ✗ separate window | Aki-style embedded console, live output, autoscroll | Launcher | Original |
| Plugin management | CLI (`dsh plugin`) | Graphical list + ~140 built-in capability rows + one-click update | Launcher | Original |
| Plugin market | ✗ | Live npm search, one-click install, jump to project page | Launcher (npm registry API) | Original |
| Skill market | ✗ | Live GitHub search, one-click install into `~/.dsh/skills` | Launcher (GitHub API) | Original |
| Community skin market | ✗ | npm skin packages are **converted in place to local CSS** and managed on the Skins page (switch/delete/import) — never leaking into the plugin system | Launcher | Converter original; skin content belongs to its authors (e.g. the [@linxin666 collection](https://github.com/zhu1090093659/dsh-web-ui)) |
| Token analytics | ✗ | GitHub-style heatmap, streaks, per-model split, daily detail | Launcher (reads cost-meter-plus ledger) | Original |
| Costs / balances | ✗ | Live balances (OpenRouter/OpenAI/local), auto price catalog sync, cache-hit strip | Plugin `dsh-cost-meter-plus` | Fork of [Han-1413141/dsh-cost-meter](https://github.com/Han-1413141/dsh-cost-meter) (MIT) |
| Usage panel, de-peaked | - | Removes peak/off-peak price display | Plugin `dsh-token-usage-plus` | Fork of [Tastelessor/dsh-usage-stats](https://github.com/Tastelessor/dsh-usage-stats) (MIT) |
| Destructive-command blocking | ask-confirm only | 38 pattern classes denied outright (`rm -rf`, format, registry, …) | Plugin `dsh-safe-guard` | Original |
| Control Deck (ST-grade) | ✗ | See the section below | Plugin `dsh-control-deck` | Original; semantics aligned with [SillyTavern](https://github.com/SillyTavern/SillyTavern) (behavior reference, no code included) |
| Frontend skin injection | ✗ | Injects `~/.dsh/frontend-skin.css` into the dsh web UI | Plugin `dsh-skin-loader` | Original |
| Hover model prices | ✗ | Model picker shows input/output USD per million tokens on hover | Plugin `dsh-price-hint` | Original |
| Quick workspace | manual GUI steps | One-click HTTP creation by absolute path | Plugin `dsh-quick-workspace` | Original |
| AI skin studio (one-shot skins) | ✗ | Ask the agent for a launcher/frontend skin in chat: it gathers requirements first (style/colors/light-dark/background art), **asks the user for an image instead of fabricating one** when it cannot generate images (or calls an image-generation tool when it can), inlines local art as data URIs, and installs+applies in one call | Plugin `dsh-skin-studio` (registers the `skin_studio` tool) + dsh skill `skin-studio` | Original |
| Engineering skills | ✗ | Architecture / plugin / frontend / ops / playbook / testing six-pack | Claude Code skills | Original |
| **Core rewrites** | - | **0 lines** — everything above goes through official extension points | - | - |

## ✦ Control Deck (brief)

> "Control Deck" page in the launcher sidebar. Every save **hot-reloads within 1.5 s** — no dsh restart. Semantics match SillyTavern, so ST veterans feel at home instantly.

1. **Prompt injection (multi-entry, leveled)** — each entry: name/text, `order` (stacking), `position` (`system` / `user-prefix`), `interval` (1 = every step), `enabled` toggle;
2. **Regex scripts (ST runRegexScript semantics)** — `findRegex` + `replaceString` with `{{match}}` and `$1…$9` capture groups, `trimStrings`, `placement` (`user_input` / `world_info`);
3. **World Info (full ST field set)** — keyword/regex keys with `andAny/andAll/notAny/notAll` logic, constant 🔵 entries, probability gates, whole-word matching (CJK-aware), recursion control, inclusion groups with weights, sticky/cooldown/delay, global `scanDepth` + `budgetChars`;
4. **Sampling overrides (with a master switch)** — off by default so nothing can leak into requests; when enabled: temperature, maxTokens, stop sequences (≤4). Reasoning effort stays with the native model picker;
5. **Tool switches** — deny tools at `tools/pre-execute` (e.g. disable `web_search`).

## ✦ Launcher page tour

| Page | What it does |
|---|---|
| Dashboard | Status (RUNNING flip effect), core & Node versions, default model, start (lightning FX) / stop |
| Plugins | Installed plugins + built-in capability list, market search & install, one-click update |
| Skills | Local skill list + GitHub market install |
| Sessions | Browse all sessions with message counts, jump to their storage |
| Storage | Sizes per store, one-click open |
| Update | One-click git pull + build for core / plugins, live progress log |
| Tokens | Totals & hit-rate overview, GitHub-style heatmap, streaks, per-model, daily detail |
| Skins | Launcher skins and dsh frontend skins managed separately: switch / delete / CSS import / community market |
| Control Deck | The ST-grade editor described above |
| Quick workspace | Create a workspace from an absolute path (dsh must be running) |
| Logs | Launcher internals / dsh output / update logs |

The 🌐 image at the bottom-left: **single-click** switches zh/en (sweep animation), **double-click** toggles light/dark.

## ✦ Plugin roster

| Plugin | Responsibility |
|---|---|
| `dsh-control-deck` | ST-grade prompts/regex/lorebook/sampling (pure-function engine + thin host shell) |
| `dsh-safe-guard` | Destructive-command denial (deny, no confirm noise) |
| `dsh-cost-meter-plus` | Balances / prices / cache hits / ledger |
| `dsh-token-usage-plus` | De-peaked usage panel |
| `dsh-skin-loader` | Frontend skin injection |
| `dsh-price-hint` | Hover model prices |
| `dsh-quick-workspace` | HTTP quick workspace |
| `dsh-skin-studio` | Model-facing skin studio: the `skin_studio` tool plus a guided requirements flow |
| `dsh-skin-center` | Local rebrand of @linxin666's skin center — 16 bundled skins, switchable on the Skins page |

## ✦ Engineering skills (six-pack)

Copy the folders under `skills/` into `~/.claude/skills/` and Claude Code masters the dsh codebase:
`dsh-architecture` (Cordis model) · `dsh-plugin-dev` (plugin contract) · `dsh-frontend-dev` (client slots) · `dsh-env-ops` (environment ops) · `dsh-playbook` (tuning playbook) · `dsh-testing` (test layering).

## ✦ Why zero core rewrites work

dsh is built on the Cordis plugin framework; the suite only uses these **official extension points**:

- `ctx.systemPrompt.section()` — sectioned system-prompt injection;
- `agent/pre-step` — rewrite messages entering the model (regex / world info / prefixes);
- `agent/request` — merge request parameters (sampling overrides);
- `tools/pre-execute` — allow/deny tool calls (safety blocking / tool switches);
- `ctx.webServer.register()` — mount HTTP endpoints (quick workspace / skin serving);
- the client `__ModuleLoader__` slot — frontend injection (skins / price hints).

The launcher is a fully separate process that talks to the core only via CLI and HTTP.

## ✦ FAQ

**Q: A task failed with `MISSING_CREDENTIAL: deepseek-official` — is that a bug?**
No. dsh sessions **pin the model chosen at creation time**. If a session was created on the official DeepSeek model while you only configured an OpenRouter key, that session keeps using the official channel and reports the missing credential. Fix: start a new session, switch models inside the session, or add `DEEPSEEK_API_KEY`.

**Q: Where did my market-installed skin go?**
The "dsh frontend skins" section of the Skins page — never the plugin list.

**Q: UI changes not showing?**
Browser cache — hard-refresh with `Ctrl+F5`.

**Q: Do Control Deck edits need a restart?**
No — saves hot-reload within 1.5 s.

## ✦ Skin system & artwork notice

- **Launcher skins** (`launcher/skins/launcher/`): `cyberpunk-2077` (Cyberpunk 2077 × Edgerunners, Jimeng-AI-generated art), `default` (three-state theme), and **`onimai`** (the 绪山真寻-themed beauty-rework skin, artwork from the official onimai.jp website — bundled for **non-commercial use only**);
- **dsh frontend skins** (`launcher/skins/frontend/`): injected into the dsh web UI via the `dsh-skin-loader` plugin. Pick "(none)" to restore stock looks;
- **Community skin market**: search npm skin packages and install with one click (double-filtered for the dsh ecosystem + skin semantics); each package is **converted in place into a single local CSS file** and managed on the Skins page — skins never end up in the plugin system. Converted files are git-ignored (copyright stays with the original authors);
- **16 bundled skins** ship inside `dsh-skin-center`, each with its own LICENSE / NOTICE.

## ✦ License & credits

Original suite code is MIT (see [LICENSE](LICENSE)); this beauty rework by **bailing** (onimai skin CSS, theme logic, rebrand) is also MIT. Forked plugins keep their upstream MIT licenses: [dsh-cost-meter](https://github.com/Han-1413141/dsh-cost-meter) (Han-1413141), [dsh-usage-stats](https://github.com/Tastelessor/dsh-usage-stats) (Tastelessor). Control Deck semantics align with [SillyTavern](https://github.com/SillyTavern/SillyTavern) (behavior reference only, no code included). Community skin content belongs to its authors (Maid Atelier is CC BY-NC-SA 4.0, non-commercial). The onimai skin's artwork comes from the official onimai.jp website and is bundled for **non-commercial use only** (copyright stays with its owners). `dsh-skin-center` is a local rebrand of [@linxin666/dsh-client-ui-skin-center](https://github.com/zhu1090093659/dsh-web-ui) (Apache-2.0). The ZCOOL KuaiLe font is licensed under the ZCOOL Font Free Commercial License. Launcher artwork generated with Jimeng AI by the suite author. Launcher UX pays homage to [秋叶 aaaki's ComfyUI pack launcher](https://space.bilibili.com/12566101).
