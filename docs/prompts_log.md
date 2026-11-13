# Prompts Log

- 日期：2026-11-13
- 工具：Claude Code（模型 Claude Opus 5.5）；OpenSpec CLI 2.0.0
- 原始 prompt：使用者提供的「任務：DIC3 — Project Proposal Refactor using OpenSpec」完整指令（第 0–8 節；要求包含唯讀來源、OpenSpec 流程、A–F／G–L／M–O 重構與引用查證規則）。對話中另加兩句：「範例檔案我都先放在DIC-3這個資料夾了 如果要移動你在自己移動」、「都不可以影響我給你的github內容」。

## 執行步驟

| # | 步驟 | 做了什麼 | 產出 |
|---|---|---|---|
| 1 | 讀懂來源 | 將兩個 GitHub repo clone 到 `/tmp/dic3src/`（唯讀）；讀 README、DESIGN、research/*、openspec/*；以 PyMuPDF／zip 解析 PDF、DOCX、PPTX 轉成文字 | 10 行專案理解與缺口清單（回報給使用者） |
| 2 | 發現 | 來源 HEAD 已做過一次 DIC3 重構；真正的重構前版本是 tag `before-dic3-20261112` | [原專案 tag before-dic3-20261112](https://github.com/benson103081/energy-detective-agent/tree/before-dic3-20261112)、[原專案 HEAD 873b14f](https://github.com/benson103081/energy-detective-agent/tree/873b14f) |
| 3 | 整理範例 | DIC-3 資料夾的 PDF／PPTX／DOCX 移到 `reference/`，只提交文字擷取 | `reference/` |
| 4 | 安裝 OpenSpec | `npm install -g @fission-ai/openspec@latest` → 2.0.0；`openspec init --tools claude --language zh-TW` | `openspec/`、`.claude/skills/` |
| 5 | 查證引用（子代理 1） | 逐筆查證來源 A1–A5、D1–D9；主代理再親自抽查 A5（arXiv） | 修正 A5 標題、A4 會議、D7 DOI、D8 日期等 |
| 6 | 新文獻（子代理 2） | 搜尋 2025–2026 的 AI 與領域文獻、臺灣統計；主代理親自抽查 A6、A7、A8、A10、D10、D13、D14，用 Crossref／DataCite 驗證 DOI，並用官方 CSV 重新計算臺灣占比 | `docs/references.md` |
| 7 | Specs | 依八大模組撰寫 9 個 capability 的 spec delta；Flowchart 每個分支轉為 Scenario，Pseudocode 01–44 對應到 Requirement | `openspec/changes/convert-design-to-workflow/` |
| 8 | 文件 change | `refactor-abstract-a2f`／`-introduction`／`-related-work`，以 `proposal-document` capability 寫成文件驗收契約 | 3 個 change |
| 9 | 文件 | docs/00–07 | `docs/` |
| 10 | 驗證與封存 | `openspec validate --all --strict` → 依序 `openspec archive -y` → 再驗證 | `docs/validation_log.md` |
| 11 | 自我檢查 | 用 grep 檢查 A–F 標題、M／N／O、摘要表列數、每個引用是否都列在 references | `docs/self_check.md` |
| 12 | git | 本 repo 分 5 次 commit；未 push；確認來源 clone 的 `git status` 乾淨 | git log |

## 子代理 prompt 摘要

1. **Verify existing proposal citations：** 「逐項到原始來源查證 D1–D9、A1–A5 的作者、年份、標題、出處、DOI 與數字，不得虛構；查不到就標 UNVERIFIED，並附支持的原文片段。」
2. **Find 2025–2026 related-work papers and stats：** 「找至少 6 篇 AI 與 6 篇領域文獻（優先 2025–2026），以及 2025–2026 的動機數據（含臺灣官方統計）。每筆都必須實際開啟過，並填 Problem／AI Method／Domain Knowledge／Dataset／Findings／Limitation；同時標註文獻在三項爭論中站哪一邊。」

子代理找到但主代理未親自核對的文獻（AnomaMind、PILLM 等），一律未納入正文，見 `docs/references.md`「本次刻意不採用的項目」。
