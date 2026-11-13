# domain-knowledge Specification

## Purpose
以可檢查的條件式規則（而非不可檢查的提示詞）整合建築能源領域知識，依資料前提決定規則是否適用，並把結果限定為需人工查核的假說。

## Requirements

### Requirement: Versioned rule registry
每條領域規則 SHALL 以 rule_id、版本、來源、前置條件、支持證據、缺少證據與查核建議一起保存；規則參數（如溫度分位數、零值長度）SHALL 於測試前固定。
（DESIGN.md §Domain Knowledge、§查詢、工具與報告契約補充）

#### Scenario: Rule output traceable
- **WHEN** apply_domain_rules 套用任一規則
- **THEN** 輸出包含 rule_id 與版本，且可追溯到所依據的事件證據 ID

### Requirement: Precondition-based eligibility
系統 SHALL 依事件、天氣、作息與時區狀態計算 eligible 規則集合；只有 eligible 規則可被 Agent 選用。
（Pseudocode 32、36）

#### Scenario: Eligibility computed
- **WHEN** 天氣已對齊但無使用者作息
- **THEN** eligible 包含 historical_deviation 與 weather_context，不包含 schedule_check

### Requirement: Historical deviation rule
historical_deviation 規則 SHALL 以同星期、同小時基準描述偏離與持續時間，並註明歷史型態不等於正常標籤。

#### Scenario: Weekday-hour deviation
- **WHEN** 事件為週三 22–02 時連續偏高
- **THEN** 規則輸出「偏離歷史同時段型態」與建議核對活動或運轉紀錄，並附限制說明

### Requirement: Schedule check requires user schedule
schedule_check 規則 SHALL 僅在使用者提供作息且當地時間可靠時啟用；MUST NOT 自行假設週末或夜間為停工時段。

#### Scenario: Schedule missing
- **WHEN** 使用者未提供營運作息
- **THEN** schedule_check 被停用，報告不出現「非營業時間浪費」字樣

### Requirement: Weather context is background only
weather_context 規則 SHALL 以訓練期氣溫分位數（暫定第 90／10 百分位）描述高／低溫背景，只建議查核空調紀錄，MUST NOT 把共現寫成因果或判定空調故障。

#### Scenario: Hot day high load
- **WHEN** 偏高事件同時氣溫高於訓練期第 90 百分位
- **THEN** 規則輸出「高溫可能提供背景，建議查核空調運轉紀錄」，不輸出「空調故障」

### Requirement: Zero-run check
zero_run_check 規則 SHALL 在至少 6 個相鄰有效逐時零值時觸發，先建議核對資料傳輸、建築停用或供電紀錄，MUST NOT 直接判定斷電。
（評估案例 C02）

#### Scenario: Long zero run
- **WHEN** 測試期出現連續 8 小時有效零值
- **THEN** 規則觸發並列出通訊、停用、供電三類待查假說

### Requirement: Rule tool failure degrades safely
可選規則工具失敗時，系統 SHALL 記錄該規則不可用，只以既有分析證據產生受限報告，MUST NOT 虛構規則結果。
（DESIGN.md Algorithm 1 說明段）

#### Scenario: Rule tool error
- **WHEN** apply_domain_rules 回傳 error
- **THEN** 報告只含分析證據與「規則不可用」限制說明，不含任何規則假說
