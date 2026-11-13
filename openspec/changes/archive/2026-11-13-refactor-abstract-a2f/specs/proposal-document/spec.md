# Spec Delta：proposal-document（Abstract）

## Purpose
定義 Energy Detective 提案文件本身的驗收契約：各章節須符合課程 A–Z 結構、每個數字與文獻可查證、且不得把預期成效寫成已完成的實驗結果。

## ADDED Requirements

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
