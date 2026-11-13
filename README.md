# DIC3：Project Proposal Refactor using OpenSpec — Energy Detective

本 repo 是 DIC3 作業：以 **OpenSpec（spec-driven development）** 重構「Energy Detective：建築用電異常調查 AI Agent」的專題提案。

- **來源專案（唯讀，未做任何修改）：** https://github.com/benson103081/energy-detective-agent
  - 重構前基準：tag `before-dic3-20261112`（commit `ab6b492`）→ [原專案 tag before-dic3-20261112](https://github.com/benson103081/energy-detective-agent/tree/before-dic3-20261112)
  - 來源 repo 自己的第一次 DIC3 版本：HEAD `873b14f` → [原專案 HEAD 873b14f](https://github.com/benson103081/energy-detective-agent/tree/873b14f)（僅供對照）
- **老師範例（唯讀，只參考結構）：** https://github.com/huanchen1107/2026-11-11-DIC3-ProjectRefactor
- **狀態：** 只有提案文件與規格。**Agent 程式與實驗尚未實作**，所有成效數字都是 expected／target。

## 資料夾結構

本 repo 只收錄繳交內容。重構前原文請點下方連結到原專案的 tag 查看；`openspec init` 產生的 Claude Code 技能檔與課程參考資料留在本機完整版，不放進本 repo。

```
.
├── README.md                     ← 本檔：作業說明與 checklist
├── docs/
│   ├── 00_openspec_notes.md      ← SDD／OpenSpec 筆記（口試用）
│   ├── 01_abstract_A2F.md        ← Abstract A–F
│   ├── 02_deep_research.md       ← 5 cores／3 trade-offs／10 details
│   ├── 03_introduction.md        ← Introduction A–F ↔ G–L＋before/after
│   ├── 04_related_work.md        ← Related Work M／N／O＋16 篇摘要表＋before/after
│   ├── 05_abstract_300.md        ← 約 300 字中文摘要
│   ├── 06_abstract_ieee.md       ← IEEE 風格英文摘要
│   ├── 07_design_to_openspec_mapping.md ← DESIGN.md → spec 對照表
│   ├── references.md             ← 全部引用＋查證日期＋[UNVERIFIED]
│   ├── validation_log.md         ← openspec validate／archive 實際輸出
│   ├── prompts_log.md            ← 使用的 prompt 與每步紀錄
│   └── self_check.md             ← 作業五點完成狀態與待人工確認事項
└── openspec/
    ├── config.yaml               ← 專案脈絡（2.0.0 用它取代 project.md）
    ├── specs/<10 capabilities>/spec.md  ← source of truth
    └── changes/
        ├── implement-mvp-agent/  ← 刻意未封存：期末實作任務
        └── archive/2026-11-13-*  ← 已完成的 4 個 change
```

## 五項作業要求 Checklist

| # | 作業要求 | 對應檔案 | 狀態 |
|---|---|---|---|
| 1 | 安裝 OpenSpec 並初始化 | `openspec/config.yaml`、`docs/00_openspec_notes.md` | ✅ 2.0.0，`--tools claude` |
| 2 | 釐清專案內容（Abstract A–F、Deep Research、300 字摘要、IEEE 摘要） | `docs/01`、`docs/02`、`docs/05`、`docs/06` | ✅ |
| 3 | DESIGN.md → OpenSpec workflow | `openspec/specs/*`（9 個系統 capability：八大模組＋offline-evaluation）、`openspec/changes/archive/*`、`docs/07`、`docs/validation_log.md` | ✅ strict 驗證通過並已 archive |
| 4 | Introduction refactor | `docs/03_introduction.md`（A–F＋G–L 對照表＋before/after） | ✅ |
| 5 | Related Work refactor | `docs/04_related_work.md`（M／N／O、16 篇摘要表、四段式收斂、before/after） | ✅ |
| — | 引用與證據 | `docs/references.md`（25 筆，查證日期 2026-11-13；[UNVERIFIED] 4 項，正文未使用其數字） | ✅ |

## 八大模組 ↔ capability

| 課程模組 | capability | 需求數 |
|---|---|---|
| ① User/Application Scenario | `user-scenario` | 3 |
| ② Query/Input | `query-input` | 5 |
| ③ Planner & Reasoning Agent | `planner-reasoning` | 6 |
| ④ Data Acquisition/Retrieval | `data-retrieval` | 9 |
| ⑤ Analytics Engine | `analytics-engine` | 7 |
| ⑥ Domain Knowledge Integration | `domain-knowledge` | 7 |
| ⑦ Decision/Recommendation | `decision-recommendation` | 4 |
| ⑧ Output & Interaction | `output-interaction` | 6 |
| （來源 DESIGN.md 的⑧離線評估） | `offline-evaluation` | 6 |
| （提案文件本身的驗收契約） | `proposal-document` | 11 |

## 如何重跑驗證

需要 Node.js ≥ 22.12.0。

```bash
npm install -g @fission-ai/openspec@2.0.0   # 本次實際版本
openspec --version                            # 2.0.0
openspec list --specs                         # 10 個 capability
openspec list                                 # 只剩 implement-mvp-agent
openspec validate --all --strict              # 預期：11 passed, 0 failed
openspec validate --archived                  # 預期：4 passed
openspec show query-input --type spec         # 檢視單一規格
```

`validate` 只檢查規格格式，不代表系統已實作。
