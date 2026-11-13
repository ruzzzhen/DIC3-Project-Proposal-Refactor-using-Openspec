# Proposal：convert-design-to-workflow

## Why

來源專案的 `DESIGN.md`（[原專案 DESIGN.md（tag before-dic3-20261112）](https://github.com/benson103081/energy-detective-agent/blob/before-dic3-20261112/DESIGN.md)）已有完整架構、Flowchart 與 44 行 Pseudocode，但它是敘述文件，無法逐條驗收；來源 HEAD 的第一版 OpenSpec 只把它濃縮成 1 個 capability、12 條需求，且未對齊課程規範的八大模組（① User Scenario → ⑧ Output & Interaction）。本 change 把 DESIGN.md 轉成以八大模組為邊界的 OpenSpec 規格，讓每個流程分支與 Pseudocode 行號都有可測的 Requirement／Scenario。

## What Changes

- 新增 8 個對應課程八大模組的 capability，另新增 `offline-evaluation`（來源 DESIGN.md 的模組⑧「離線評估」不在課程八大模組內，獨立成 supporting capability）。
- Flowchart 的每個判斷菱形（MV、QV、PV、PR、LV、RET、DV、W、AV、CH、RV）轉為 Scenario；迴圈（計畫修正一次、I/O 重試一次）與例外（FAIL、INSUF、模板降級）各有 Scenario。
- Pseudocode 01–44 每一行對應到某個 Requirement，對照表見 `docs/07_design_to_openspec_mapping.md`。
- 不改動 DESIGN.md 的任何設計決策；所有門檻（90%、P99、10 次呼叫等）照原文標為暫定設定。

## Capabilities

### New Capabilities
- `user-scenario`：① 使用者、調查目標與應用邊界
- `query-input`：② 自然語言需求解析與範圍驗證
- `planner-reasoning`：③ 受限規劃、白名單、預算、重試與依觀察決策
- `data-retrieval`：④ 資料清單、重播邊界、raw 資料與品質閘門
- `analytics-engine`：⑤ 時間切分、B0／B1 基準、校準門檻與事件合併
- `domain-knowledge`：⑥ 條件式領域規則與前提檢查
- `decision-recommendation`：⑦ 五段式建議與推論邊界
- `output-interaction`：⑧ 證據引用報告、驗證、模板降級與執行紀錄
- `offline-evaluation`：預定評估協議（B0/B1、合成注入、C01–C10、三組比較）

### Modified Capabilities
無（本 repo 尚無正式 specs）。

## Impact

- 只影響 `openspec/` 規格與 `docs/07_design_to_openspec_mapping.md`；不新增程式碼。
- archive 後 `openspec/specs/` 成為「預定系統行為」的 source of truth；系統實作另列於未封存的 change `implement-mvp-agent`。
