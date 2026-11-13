# Self Check

## 作業五點完成狀態

| # | 要求 | 狀態 | 證據 |
|---|---|---|---|
| 1 | 安裝 OpenSpec、init（Claude Code）、填專案脈絡、SDD 筆記 | ✅ | `openspec --version` = 2.0.0；`openspec/config.yaml`；`docs/00_openspec_notes.md` |
| 2 | Abstract A–F、Deep Research 5/3/10、300 字摘要、IEEE 摘要 | ✅ | `docs/01`（6 個標題）、`docs/02`（10 題涵蓋 8 種推理層次）、`docs/05`（約 330 字，不含引用標記）、`docs/06`（257 words） |
| 3 | DESIGN.md → specs（每模組一個 capability）、4 個 change、strict 驗證、archive、對照表 | ✅ | 10 個 specs、64 條 Requirement；`validate --all --strict` 11/11；`validate --archived` 4/4；`docs/07` |
| 4 | Introduction A–F＋G–L 對照＋before/after | ✅ | `docs/03`：D 段為 7 列表格；文末附字母對照表與 before/after |
| 5 | Related Work：AI／領域各半、M／N／O、≥8 篇摘要表、四段收斂、before/after | ✅ | `docs/04`：16 篇（2025–2026 年 11 篇） |
| — | 引用規則 | ✅ | `docs/references.md` 共 25 筆，查證日期皆為 2026-11-13；正文用到的每個 [A*]／[D*] 都能在 references 找到（已用 grep 檢查） |

## 與規範的偏離（需要知道並能解釋）

1. **9 個系統 capability，而不是 8 個：** 來源 DESIGN.md 的⑧是「離線評估」，課程的⑧是「Output & Interaction」。評估協議獨立成 `offline-evaluation`。
2. **多一個 `proposal-document` capability：** 3 個文件 change 用它寫下可驗收的文件規則。OpenSpec 2.0.0 對純文件變更的建議其實是 `skip_specs: true`；理由見 `docs/00` §6。
3. **`project.md` 不存在：** 2.0.0 已改用 `config.yaml` 的 `context`。
4. **300 字摘要約 330 字：** 不含引用標記。
5. **Flowchart 與 Pseudocode 的一處不一致：** CH 違反前置條件時，Flowchart 走 FAIL，Pseudocode 第 34 行走 RESTRICTED_REPORT。spec 的處理方式見 `docs/07` §E。

## [UNVERIFIED] 與需要人工確認的地方

1. D7 LBNL 報告的作者順序：LBNL 頁面與 DataCite 的作者列表不一致，報告編號也沒找到。
2. A4 的會議名稱（2025 ASHRAE Annual Conference）：來自 LBNL 書目，全文 PDF 沒有標示。
3. D10 LEAD1.0 的正式發表處：只以 arXiv preprint 引用。
4. A9 與 D12 的內容摘要：書目資料已用 Crossref 確認，但內容取自研究代理讀到的摘要，本人沒有開全文。正文只做質性引用，不使用數字。
5. D15 臺灣占比：本專題自行由官方 CSV 加總計算，不是能源署公布的占比；近期月份可能修正。
6. 目標使用者（設施管理者）：沿用來源的情境設定，尚未經過訪談驗證。
7. git 身分：本 repo 的 local config 設為 `nlpnchu <nlpnchu@gmail.com>`。如需改用你本人的姓名，請執行 `git config user.name "..."`。
8. 本 repo 只放繳交內容（README、docs、openspec）；重構前原文改以連結指向原專案 tag `before-dic3-20261112`。
