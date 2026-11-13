# Spec Delta：proposal-document（Related Work 與引用）

## ADDED Requirements

### Requirement: Balanced AI and domain coverage
Related Work SHALL 約一半篇幅討論 AI Technology、一半討論 Domain Know-how，並依 M（分類，由舊到新、由簡到繁）、N（新提出）、O（優缺點比較表）組織。

#### Scenario: M-N-O sections present
- **WHEN** 審閱者打開 `docs/04_related_work.md`
- **THEN** 看到 M、N、O 三節，且 AI 與領域兩類各有獨立小節

### Requirement: Article summary table
Related Work SHALL 提供至少 8 篇文獻的摘要表，欄位為 Ref、Year、Problem、AI Method、Domain Knowledge、Dataset、Findings、Limitation，並優先納入 2025–2026 文獻。

#### Scenario: Table size
- **WHEN** 計算摘要表列數
- **THEN** 至少 8 列，且多數列的 Year 為 2025 或 2026

### Requirement: Research gap convergence
Related Work SHALL 以 Existing Research → Limitations → Research Gap → Our Project 收斂，MUST NOT 宣稱演算法首創或所有文獻皆未處理。

#### Scenario: Gap statement
- **WHEN** 審閱者閱讀結尾
- **THEN** 能看到四段式收斂，且研究缺口以「本專題選定的驗證問題」措辭表述

### Requirement: Every citation verified
每個數字、統計與文獻 SHALL 在 `docs/references.md` 列出作者、年份、標題、出處、DOI 或 URL 與查證日期；查不到原始來源者 MUST 刪除或標記 `[UNVERIFIED]` 並集中列於該檔末尾。

#### Scenario: Unverifiable statistic
- **WHEN** 某數字找不到原始來源
- **THEN** 正文刪除該數字或標記 [UNVERIFIED]，且該項列在 references.md 最下方
