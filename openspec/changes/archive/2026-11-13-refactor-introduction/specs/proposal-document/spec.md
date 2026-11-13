# Spec Delta：proposal-document（Introduction）

## ADDED Requirements

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
