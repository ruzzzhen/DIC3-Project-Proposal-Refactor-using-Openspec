# Design：convert-design-to-workflow

## Context

輸入是 [原專案 DESIGN.md（tag before-dic3-20261112）](https://github.com/benson103081/energy-detective-agent/blob/before-dic3-20261112/DESIGN.md)（重構前基準）。它的模組編號（①需求解析…⑧離線評估）與課程規範的八大模組（①User Scenario…⑧Output & Interaction）不同，需要重新對位。

## Goals / Non-Goals

**Goals:**
- 每個 capability 對應課程八大模組之一，Requirement 一條只描述一個可觀察行為。
- Flowchart 判斷、迴圈、例外全部轉為 Scenario；Pseudocode 每行可追溯到 Requirement。

**Non-Goals:**
- 不實作 Agent、不下載 BDG2、不產生任何實驗數字。
- 不調整原設計的門檻或流程；若原文前後不一致，只記錄並採較保守解讀。

## Decisions

1. **模組對位**：課程①↔原 Problem/Requirements；②↔原①；③↔原②；④↔原③＋④（資料取得與品質閘門合併為「取得與前處理」）；⑤↔原⑤；⑥↔原⑥；⑦↔原⑦的「建議與推論邊界」；⑧↔原⑦的「報告、驗證、保存」。原⑧離線評估獨立為 `offline-evaluation`。
   - 替代方案：把評估塞進 ⑧ Output。捨棄原因：評估是實驗協議，不是使用者可見的輸出行為。
2. **Spec 描述行為而非實作**：不寫 `src/agent.py` 等檔名；工具名（load_window 等）屬外部可觀察介面，保留。
3. **不一致處理**：Flowchart 中「CH 違反前置條件 → FAIL」與 Pseudocode 34 行「RESTRICTED_REPORT_WITH_LOG」不同；DESIGN.md 正文寫「停止或以已驗證證據產生受限報告」。spec 採「停止並以已驗證證據產生受限報告＋失敗紀錄」，同時滿足兩者。
4. **Pseudocode 行號**：以 DESIGN.md 的 01–44 版本為準（proposal_source.md 另有 01–37 精簡版，對照表一併列出）。

## Risks / Trade-offs

- [規格看起來像已實作] → config.yaml 與 README 明示 specs 為「預定系統」契約，implement-mvp-agent 保持未封存。
- [九個 capability 過細] → 以課程評分要求的模組邊界為準，便於口試逐模組說明。
