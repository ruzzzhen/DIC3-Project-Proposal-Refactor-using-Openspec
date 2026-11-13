# 00 Spec-Driven Development 與 OpenSpec 筆記

> 依本機實際安裝版本撰寫：`openspec --version` → **2.0.0**（Node.js v22.20.0，套件要求 `>=22.12.0`）。指令與目錄以 `openspec --help` 實際輸出為準。

## 1. 一句話說明

**Spec-driven development（SDD）＝先把「系統應該做什麼」寫成可驗收的規格，人和 AI 對規格達成共識之後才動手；規格而不是聊天紀錄，才是 source of truth。**
OpenSpec 是一個輕量 CLI，把這件事落在 repo 裡的 `openspec/` 資料夾：用 Markdown 寫規格，用 `openspec validate` 檢查格式，用 `archive` 把完成的變更併回正式規格。

## 2. 目錄結構（2.0.0 實際產生）

```
openspec/
├── config.yaml            # 專案脈絡（舊版叫 project.md；2.0.0 改用 config.yaml 的 context 欄位）
├── specs/                 # 正式規格＝目前系統「是什麼」(source of truth)
│   └── <capability>/spec.md
└── changes/               # 進行中的變更＝打算「改成什麼」
    ├── <change-name>/
    │   ├── .openspec.yaml # schema、建立日期；可設 skip_specs: true
    │   ├── proposal.md    # Why / What Changes / Capabilities / Impact
    │   ├── design.md      # 技術決策、替代方案、風險（需要時才寫）
    │   ├── tasks.md       # 勾選清單
    │   └── specs/<capability>/spec.md   # spec delta：ADDED/MODIFIED/REMOVED/RENAMED
    └── archive/YYYY-MM-DD-<change-name>/  # 已完成的變更
.claude/skills/openspec-*  # openspec init --tools claude 產生的 Claude Code 技能（propose/apply/archive…）
```

## 3. 四步流程：propose → review → apply → archive

| 步驟 | 做什麼 | 本次使用的指令 | 產物 |
|---|---|---|---|
| **Propose** | 開一個 change，寫清楚為什麼改、改什麼、影響哪些 capability | `openspec new change <name>`；`openspec instructions proposal --change <name>` | proposal.md、spec delta、design.md、tasks.md |
| **Review** | 人檢查 proposal 與 spec delta 是否正確；機器檢查格式 | `openspec status --change <name>`；`openspec validate <name> --strict`；`openspec show <name>` | 驗證通過的 change |
| **Apply** | 依 tasks.md 實作並逐項打勾 | （Claude Code 技能 `/openspec-apply-change`）本次是「文件」實作：撰寫 docs/01–07 | 勾完的 tasks.md |
| **Archive** | 把 spec delta 合併進 `openspec/specs/`，change 移到 archive | `openspec archive <name> -y` | 更新後的正式 specs |

口訣：**propose 寫意圖、review 達共識、apply 照規格做、archive 讓規格成為真相。**

## 4. specs vs changes（口試最常問）

| | `specs/` | `changes/` |
|---|---|---|
| 意義 | 目前（或本專案：預定）系統行為的**唯一真相** | 對真相的**修改提案** |
| 寫法 | 完整需求：`## Purpose`＋`## Requirements` | 只寫差異：`## ADDED / MODIFIED / REMOVED / RENAMED Requirements` |
| 生命週期 | 長期存在，只透過 archive 改變 | 短期存在，完成後移到 `archive/` |
| 類比 | git 的 main branch | 一個 pull request |

規格格式規則（`--strict` 會檢查）：
- `### Requirement: <名稱>`，內文用 **SHALL / MUST**，一條只講一個行為，描述 ≤ 500 字元。
- 每條 Requirement 至少一個 `#### Scenario:`（**必須四個 #**），內含 **WHEN** 與 **THEN**。
- 新 capability 的 delta 要有 `## Purpose`（≥ 50 字元）。
- 每個 change 必須至少有一個 delta，否則要在 `.openspec.yaml` 設 `skip_specs: true`（純工具或文件變更）。

## 5. 這次作業我怎麼用 OpenSpec

1. `openspec init --tools claude --language zh-TW`：AI 工具選 Claude Code，並在 `config.yaml` 寫入專案目的、技術棧、領域背景與慣例。
2. **Capability 切法**：依課程八大模組切成 8 個 capability，再加 `offline-evaluation` 和 `proposal-document`：
   - 系統行為：`user-scenario`、`query-input`、`planner-reasoning`、`data-retrieval`、`analytics-engine`、`domain-knowledge`、`decision-recommendation`、`output-interaction`，以及 `offline-evaluation`
   - 提案文件本身的驗收契約：`proposal-document`
3. **四個 change**：
   - `convert-design-to-workflow`：DESIGN.md → 9 個系統 capability。Flowchart 分支轉成 Scenario，Pseudocode 行號轉成 Requirement（對照表見 `docs/07`）。
   - `refactor-abstract-a2f`、`refactor-introduction`、`refactor-related-work`：把 A–F、Introduction、Related Work 的寫作規範（例如「F 段不得虛構結果」「每個數字都要有可查證來源」）寫成 `proposal-document` 的 Requirement，讓文件品質也能逐條驗收。
4. **validate → archive**：四個 change 都以 `--strict` 驗證通過後 archive，`openspec/specs/` 就成為唯一真相。輸出記錄在 `docs/validation_log.md`。
5. **未封存的 change**：`implement-mvp-agent` 設為 `skip_specs: true`，只列實作任務。它刻意保持進行中，表示「規格已定、程式未做」。這正是 specs 和 changes 的差別：規格寫的是預定行為，change 的 tasks 才代表進度。

## 6. 口試可能的追問與回答要點

- **為什麼要 SDD，不直接叫 AI 寫？** 需求只活在聊天紀錄裡，就會漂移，AI 會自己補細節。寫成 spec 之後，人和 AI 對「完成」有同一個可檢查的定義，每個 Scenario 之後也可以直接變成測試案例。
- **validate 通過代表系統完成了嗎？** 不代表，它只檢查格式（標題、SHALL、WHEN/THEN、Purpose 長度）。行為正不正確，要等實作後逐一執行 Scenario 才知道。
- **為什麼文件 change 也寫 spec delta，而不是 skip_specs？** 這次的交付物就是提案文件。「Abstract F 段不得虛構結果」「每個數字有出處」是可以客觀檢查的驗收條件，不是為了通過驗證而硬湊的需求。如果老師認為文件變更不該有 spec，替代做法是改用 `skip_specs: true` 搭配 `archive --skip-specs`。
- **MODIFIED 和 ADDED 差在哪？** MODIFIED 必須貼上整條需求的完整新內容，因為 archive 會整條取代。ADDED 只是新增。
- **Flowchart 怎麼變成 spec？** 每個菱形判斷的每個出口都是一個 WHEN/THEN；迴圈（修正一次、重試一次）寫成「第二次失敗時」的 Scenario；例外路徑（FAIL、INSUF）寫成「不得做什麼」的 THEN。
