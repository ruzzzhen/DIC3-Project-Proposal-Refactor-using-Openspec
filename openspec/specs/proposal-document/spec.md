# proposal-document Specification

## Purpose
定義 Energy Detective 提案文件本身的驗收契約：各章節須符合課程 A–Z 結構、每個數字與文獻可查證、且不得把預期成效寫成已完成的實驗結果。

## Requirements

### Requirement: Abstract follows A–F story
Abstract SHALL 依序包含 A. Attention Getter、B. But、C. Cure、D. Development、E. Experiments、F. Findings 六段，C 段 MUST 明確寫出「本專題提出 Energy Detective AI Agent」。

#### Scenario: Six sections present
- **WHEN** 審閱者打開 `docs/01_abstract_A2F.md`
- **THEN** 依序看到 A–F 六個標題，且 C 段含有提出之系統名稱

### Requirement: Findings are expected outcomes only
F 段 MUST 只寫 Expected Outcomes／target，並明示尚未執行實驗；MUST NOT 出現以過去式陳述的實驗數字。

#### Scenario: Target written as achieved
- **WHEN** F 段出現「本系統達成 90% 命中率」這類已完成語氣
- **THEN** 該文件不通過驗收，須改為「目標／預期」並註明待測

### Requirement: Experiments name dataset, baseline and metrics
E 段 SHALL 具體列出 dataset、至少一個 baseline、評估指標與 case study 名稱。

#### Scenario: Evaluation is concrete
- **WHEN** 審閱者閱讀 E 段
- **THEN** 能找到 BDG2、B0／固定流程 baseline、MAE／注入命中率／事實可追溯率等指標與 C01–C10 案例

### Requirement: Condensed abstracts
提案 SHALL 另附約 300 字中文單段摘要與 IEEE 風格英文摘要（Title、單段 Abstract、Index Terms），兩者內容 MUST 與 A–F 一致且不新增未查證數字。

#### Scenario: IEEE abstract structure
- **WHEN** 審閱者打開 `docs/06_abstract_ieee.md`
- **THEN** 看到 Title、單一段落 Abstract 與 Index Terms 三部分

### Requirement: Introduction follows A–F and maps to G–L
Introduction SHALL 依序包含 A Attention Getter、B Background、C Problem、D Existing Limitations、E Innovation、F Contributions，並 SHALL 附表標明每段對應 A2Z 的 G General、H However、I In literature、J Judgement、K Keypoint、L List the organization。

#### Scenario: Letter mapping table present
- **WHEN** 審閱者讀到 Introduction 結尾
- **THEN** 看到一張表，G–L 六個字母皆對應到至少一段

### Requirement: Existing limitations as a table
D. Existing Limitations SHALL 以表格呈現現有方法、限制與本專題回應，每列限制 MUST 有引用或標明為本專題推論。

#### Scenario: Limitation without evidence
- **WHEN** 表格某列既無引用也未標明「本專題推論」
- **THEN** 該文件不通過驗收

### Requirement: Introduction before/after record
Introduction 重構 SHALL 附 before/after 對照，說明來源原文重點、修改內容與修改理由。

#### Scenario: Reviewer checks changes
- **WHEN** 審閱者想知道改了什麼
- **THEN** 能在對照表中找到每個 A–F 段落的原文重點、改後重點與理由

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
