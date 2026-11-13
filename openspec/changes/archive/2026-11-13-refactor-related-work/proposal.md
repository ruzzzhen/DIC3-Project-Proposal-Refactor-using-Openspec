# Proposal：refactor-related-work

## Why

來源 Related Work 只有 5 篇 AI＋5 份領域資料，其中 D1、D2、D8 為官方報告／指南、D3、D7 為 2020 年文獻，2025–2026 的同儕審查研究不足；A5 標題缺字、A4 未註明會議出處。本 change 擴充並重新查證文獻，依 A2Z 的 M／N／O 重組。

## What Changes

- 新增 `docs/04_related_work.md`：約 1/2 AI Technology＋1/2 Domain Know-how；M（由舊到新、由簡到繁分類）、N（新趨勢與本專題）、O（優缺點比較表）。
- Article Summary Table ≥ 8 篇，欄位 Ref／Year／Problem／AI Method／Domain Knowledge／Dataset／Findings／Limitation，優先 2025–2026。
- 結尾收斂 Existing Research → Limitations → Research Gap → Our Project；附 before/after 對照。
- 修正既有引用（A4 會議、A5 標題與 ACL 2026 Findings、D7 DOI、D8 更新日期）。

## Capabilities

### New Capabilities
無。

### Modified Capabilities
- `proposal-document`：新增 Related Work 結構與文獻查證需求。

## Impact

影響 `docs/04_related_work.md`、`docs/references.md`。
