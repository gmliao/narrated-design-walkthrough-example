# Narrated Design Walkthrough Architecture

- `status`: generated-derivative
- `sourceDesign`: [`docs/design/2026-05-21-narrated-design-walkthrough-architecture.md`](../../design/2026-05-21-narrated-design-walkthrough-architecture.md)
- `sourcePlan`: (none)
- `lastRegenerated`: 2026-05-21

這份 walkthrough 是 canonical design doc 的 generated derivative，用於 async review、onboarding 與架構 briefing。正式決策記錄仍以 source design doc 為準。

## 內容邊界

本目錄只應包含：

- `slides.md`：Slidev content、每張 slide 的 `<NarrationCue :text>`、speaker notes、`data-walkthrough-anchor` wiring。
- `README.md`：provenance、preview commands、validation commands、edit boundary。

Shared runtime 放在 [`docs/walkthroughs/_addon/`](../_addon/)。不要把 `NarrationCue`、`GlobalCaptions`、`TTSNavButtons`、composables 或 spotlight CSS 複製進本 walkthrough 目錄。

## Preview

所有 walkthrough 共用 root [`docs/walkthroughs/package.json`](../package.json) 與 root `node_modules`。

```bash
cd docs/walkthroughs
npm run list
npm run validate 2026-05-21-narrated-design-walkthrough-architecture
npm run dev 2026-05-21-narrated-design-walkthrough-architecture
```

預設 preview URL：

```text
http://127.0.0.1:3030
```

如果 3030 port 已被占用：

```bash
npm run dev 2026-05-21-narrated-design-walkthrough-architecture -- --port 4000
```

## Validation Checklist

請先跑 `validate` 再開 dev server。它會檢查：

- `F0-F2`：缺少 `addons: [./_addon]`，或誤寫成 `../_addon`。
- `M1`：Mermaid labels 有未 escape 的 brackets。
- `N1`：殘留舊版 per-slide `<TTSPlayer>` / `<GlobalTTSPlayer>`。

Live preview 至少確認：

- title / source boundary slide。
- 至少一張 Mermaid slide。
- 至少一張 phase / decision slide。
- Slidev bottom-left nav 裡有 TTS controls。
- 按下 Listen 後 captions 與 spotlight 行為正常。

## Edit Boundary

- 設計內容變更時，先更新 canonical source design。
- Source design 有實質變更後，再從 source 重新生成本 walkthrough。
- `<NarrationCue :text>` 和 speaker notes 必須保持同步。
- 每個 spoken stage direction 都必須同時有 `[h:anchor]...[/h]` markup 與 matching `data-walkthrough-anchor`。
- Shared playback behavior 屬於 `_addon`，不要放進這個 generated directory。
