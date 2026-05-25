# narrated-design-walkthrough-example

Reference repo demonstrating the [narrated-design-walkthrough](https://github.com/gmliao/narrated-design-walkthrough) skill. Two persona variants are showcased side by side: an **internal design** walkthrough (where the skill explains its own architecture) and an **external source** walkthrough (where the skill filters an Anthropic blog post into team-specific recommendations).

此 repo 是 [narrated-design-walkthrough](https://github.com/gmliao/narrated-design-walkthrough) skill 的展示範例。並列兩種 persona 變體：**內部設計**（skill 解釋它自己的架構）與**外部資源**（skill 把 Anthropic 部落格過濾成團隊適用的建議）。

---

## Live Demo / 線上展示

Landing page: **https://gmliao.github.io/narrated-design-walkthrough-example/**

| Walkthrough | Type / 類型 | URL |
|---|---|---|
| **Narrated Design Walkthrough Architecture** | Internal design / 內部設計 | https://gmliao.github.io/narrated-design-walkthrough-example/2026-05-21-narrated-design-walkthrough-architecture/ |
| **Claude Code in Large Codebases** | External source / 外部資源 | https://gmliao.github.io/narrated-design-walkthrough-example/2026-05-24-claude-code-large-codebases/ |

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-blue)](https://gmliao.github.io/narrated-design-walkthrough-example/)

> Browser support / 瀏覽器建議：Chrome / Edge（中文 TTS 聲音品質最佳）

---

## Two persona variants / 兩種 persona 變體

| Variant | Source type | Narrator role | Lead sentence shape |
|---|---|---|---|
| **Internal design** | `docs/design/*.md` — team's own design doc | Decision owner | "We decided X because…" / "我們決定 X，因為…" |
| **External source** | `docs/external/*.md` — snapshot of blog / RFC / talk | Filter and judgment owner | "I'd adopt X for our team because…" / "我建議我們先採用 X，因為…" |

Both share the same playback engine (TTS + spotlight + captions). The persona swap changes *what gets said*, not *how it gets rendered*. Full rules: skill's `Narration Reference → Persona Variant: External Source`.

---

## How this repo was built / 如何建立此 repo

**English:**

1. Add the skill as a Claude Code plugin:
   ```bash
   claude plugin marketplace add gmliao/narrated-design-walkthrough
   claude plugin install narrated-design-walkthrough
   ```
2. Start a Claude Code session, run `/narrated-design-walkthrough` pointing at:
   - a `docs/design/*.md` (internal design persona), or
   - a URL (external source persona — it snapshots to `docs/external/` first)
3. Push the generated output — GitHub Actions builds and deploys all walkthroughs to Pages automatically

**中文：**

1. 將 skill 加入 Claude Code：
   ```bash
   claude plugin marketplace add gmliao/narrated-design-walkthrough
   claude plugin install narrated-design-walkthrough
   ```
2. 開啟 Claude Code session，執行 `/narrated-design-walkthrough` 並指向：
   - `docs/design/*.md`（內部設計 persona），或
   - 一個 URL（外部資源 persona — 會先 snapshot 到 `docs/external/`）
3. Push 產出結果，GitHub Actions 自動建置所有 walkthroughs 並部署到 Pages

---

## Local preview / 本地預覽

```bash
cd docs/walkthroughs
npm install   # 只需執行一次 / once

# Internal design walkthrough
npm run dev 2026-05-21-narrated-design-walkthrough-architecture
# 開啟 http://localhost:3030 / open http://localhost:3030

# External source walkthrough
npm run dev 2026-05-24-claude-code-large-codebases
# 開啟 http://localhost:3030 / open http://localhost:3030
```

`npm run list` 會列出所有已收錄的 walkthrough。`npm run validate <slug>` 在開啟 dev server 之前先檢查結構（addons 路徑、Mermaid escape、deprecated TTS component）。

---

## Repo layout / 目錄結構

```
.
├── docs/
│   ├── design/          # Internal design docs / 內部設計文件 (source for internal-persona walkthroughs)
│   ├── external/        # Snapshotted external sources / 外部資源 snapshot (source for external-persona walkthroughs)
│   │   └── 2026-05-24-claude-code-large-codebases.md
│   └── walkthroughs/    # Generated Slidev presentations / 生成的 walkthrough
│       ├── _addon/      # Shared TTS + spotlight engine / 共用播放引擎
│       ├── index.html   # Landing page listing all walkthroughs / 入口頁
│       ├── 2026-05-21-narrated-design-walkthrough-architecture/   # internal design
│       │   ├── slides.md
│       │   └── README.md
│       └── 2026-05-24-claude-code-large-codebases/                # external source
│           ├── slides.md
│           └── README.md
├── .github/
│   └── workflows/
│       └── deploy-pages.yml   # Build & deploy all walkthroughs to GitHub Pages
└── README.md
```

---

## Provenance / 來源

- `2026-05-21-narrated-design-walkthrough-architecture/` — generated from `docs/design/2026-05-21-narrated-design-walkthrough-architecture.md`
- `2026-05-24-claude-code-large-codebases/` — generated from `docs/external/2026-05-24-claude-code-large-codebases.md` (snapshot of [Anthropic blog](https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start), captured 2026-05-24)

Last regenerated using narrated-design-walkthrough @ 2026-05-24

---

## License

MIT
