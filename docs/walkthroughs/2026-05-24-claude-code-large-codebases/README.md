# Claude Code in Large Codebases — Team Application Walkthrough

- `status`: generated-derivative
- `sourceUrl`: <https://claude.com/blog/how-claude-code-works-in-large-codebases-best-practices-and-where-to-start>
- `sourceSnapshot`: [`docs/external/2026-05-24-claude-code-large-codebases.md`](../../external/2026-05-24-claude-code-large-codebases.md)
- `sourceFetched`: 2026-05-24
- `lastRegenerated`: 2026-05-24
- `persona variant`: external-source (Narration Reference → "Persona Variant: External Source")

這份 walkthrough 是 Anthropic blog 的 generated derivative，用於團隊內部分享：以「資深工程師看完外部資源後，挑出對我們團隊最有用的幾點」的人設，做出可聽、可指的應用建議。**不是原文翻譯**，刻意只挑四個立刻可用的點 + 兩個延後 + 三個未解問題。

## 為什麼有這份 walkthrough

這份是 narrated-design-walkthrough skill 第二種 source 變體（external source）的首次驗證。它示範：

1. 把外部 URL 先 snapshot 到 [`docs/external/`](../../external/)，避免原文變更時 walkthrough 失去 reference。
2. 用「filter and judgment owner」persona 而非「decision owner」persona — 結論不是「我們團隊決定要 X」，而是「我看完後建議我們團隊先做 X、延後 Y」。
3. 用 `sourceUrl` / `sourceSnapshot` / `sourceFetched` 取代 `sourceDesign` / `sourcePlan`。

詳見 skill 的 Narration Reference → "Persona Variant: External Source"。

## 內容邊界

本目錄只應包含：

- `slides.md`：12 張 Slidev content、每張 slide 的 `<NarrationCue :text>`、speaker notes、`data-walkthrough-anchor` wiring。
- `README.md`：provenance、preview commands、validation commands、edit boundary。

Shared runtime 仍放在 [`docs/walkthroughs/_addon/`](../_addon/)。

## Preview

所有 walkthrough 共用 root [`docs/walkthroughs/package.json`](../package.json) 與 root `node_modules`。

```bash
cd docs/walkthroughs
npm run list
npm run validate 2026-05-24-claude-code-large-codebases
npm run dev 2026-05-24-claude-code-large-codebases
```

預設 preview URL：

```text
http://127.0.0.1:3030
```

如果 3030 port 已被占用：

```bash
npm run dev 2026-05-24-claude-code-large-codebases -- --port 4000
```

## Validation Checklist

請先跑 `validate` 再開 dev server。它會檢查：

- `F0-F2`：缺少 `addons: [./_addon]`，或誤寫成 `../_addon`。
- `M1`：Mermaid labels 有未 escape 的 brackets。
- `N1`：殘留舊版 per-slide `<TTSPlayer>` / `<GlobalTTSPlayer>`。

Live preview 至少確認：

- title / source attribution slide（slide 1）。
- 唯一一張 Mermaid slide（slide 4：Agentic search vs RAG）。
- 至少一張 phase-grid / decision slide（slide 2 或 slide 9）。
- Slidev bottom-left nav 裡有 TTS controls。
- 按下 Listen 後 captions 與 spotlight 行為正常。

## Edit Boundary

- 如果原文 URL 更新，先更新 [snapshot 檔案](../../external/2026-05-24-claude-code-large-codebases.md)，再從新 snapshot 重新產這份 walkthrough。**不要**直接照新 URL 改 slides，否則就失去「對 2026-05-24 那版的判斷」這個邊界。
- `<NarrationCue :text>` 和 speaker notes 必須保持同步（同一段內容，兩個位置）。
- 每個 spoken stage direction 都必須同時有 `[h:anchor]...[/h]` markup 與 matching `data-walkthrough-anchor`。
- Persona 是 external-source 變體：narration 用「我建議我們團隊…」/「我延後 X」/「我從原文挑出…」開頭，不要退化成「原文說…」的字幕複讀。
- Shared playback behavior 屬於 `_addon`，不要放進這個 generated directory。

## 12-slide 結構

| # | Slide | 類型 |
|---|---|---|
| 1 | Title + sourceUrl / sourceSnapshot | cover |
| 2 | 為什麼這篇對我們現在重要 | 3-card phase grid |
| 3 | 原文範圍 vs. 本 walkthrough 取捨 | 2 data-cards |
| 4 | 核心觀念：Agentic search 不是 RAG | Mermaid + 1 implication card |
| 5 | Harness 七元件 + 我給的優先級 | compact-table 7 rows |
| 6 | 建議一：CLAUDE.md hierarchy | 3-card phase grid |
| 7 | 建議二：Skills vs. CLAUDE.md 邊界 | 2 data-cards + anti-pattern note |
| 8 | 建議三：每季 review 配置 | 2 data-cards |
| 9 | 建議四：要 DRI 不要新團隊 | 3-card phase grid |
| 10 | Where to start — Monday 三件事 | step-grid 3 cards + success metric |
| 11 | Where I push back / skip | 2 data-cards + pushback rule |
| 12 | 未解問題 + 下一份該讀 | compact-table 3 rows |
