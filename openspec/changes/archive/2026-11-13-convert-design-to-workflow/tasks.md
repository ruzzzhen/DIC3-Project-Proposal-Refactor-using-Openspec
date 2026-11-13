# Tasks：convert-design-to-workflow

## 1. 準備

- [x] 1.1 將重構前 DESIGN.md 複製到 [原專案 tag before-dic3-20261112](https://github.com/benson103081/energy-detective-agent/tree/before-dic3-20261112)
- [x] 1.2 對位來源模組①–⑧與課程八大模組，記錄於 design.md

## 2. Specs

- [x] 2.1 撰寫 user-scenario、query-input、planner-reasoning spec delta
- [x] 2.2 撰寫 data-retrieval、analytics-engine、domain-knowledge spec delta
- [x] 2.3 撰寫 decision-recommendation、output-interaction、offline-evaluation spec delta
- [x] 2.4 Flowchart 所有判斷節點皆有 Scenario；Pseudocode 01–44 皆對應 Requirement

## 3. 驗證與封存

- [x] 3.1 `openspec validate convert-design-to-workflow --strict` 通過
- [x] 3.2 產出 `docs/07_design_to_openspec_mapping.md`
- [x] 3.3 archive，使 `openspec/specs/` 成為 source of truth
