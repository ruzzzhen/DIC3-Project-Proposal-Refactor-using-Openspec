# Spec Delta：⑧ Output & Interaction

## Purpose
定義調查報告的產生、數值與引用驗證、模板降級、互動回覆格式，以及每次執行（含失敗）都保存可重現紀錄的行為。

## ADDED Requirements

### Requirement: Evidence-cited report
系統 SHALL 以凍結的唯讀證據清單呼叫 build_report，產生繁體中文報告；每個數值主張 SHALL 以 evidence_id 與 field 指向證據，顯示時才四捨五入。
（Pseudocode 38–39；Flowchart R）

#### Scenario: Report cites evidence
- **WHEN** 報告提到某事件偏離 240 kWh
- **THEN** 該句附有對應 evidence_id 與欄位名，且證據中的未四捨五入值與之相符

### Requirement: Reference and number validation
系統 SHALL 比對報告中每個主張的 evidence_id、建築、期間、單位與數值；任一不符即驗證失敗，MUST NOT 只檢查 ID 是否存在。
（Pseudocode 40；Flowchart RV）

#### Scenario: Existing evidence with wrong unit
- **WHEN** evidence_id 存在但報告把 kWh 寫成 kW
- **THEN** 驗證失敗

#### Scenario: Non-existent evidence id
- **WHEN** 報告草稿引用不存在的 evidence_id
- **THEN** 驗證失敗

### Requirement: Deterministic template fallback
報告驗證失敗或 build_report 工具失敗時，系統 SHALL 以已登錄工具結果套用固定模板輸出，MUST NOT 補造結果。
（Pseudocode 41；Flowchart RV→RT→SAVE）

#### Scenario: Fallback after validation failure
- **WHEN** LLM 草稿驗證失敗
- **THEN** 最終報告由固定模板產生，並在紀錄中標示 template_fallback = true，草稿另存

### Requirement: Persist every run
系統 SHALL 對每次執行（成功、待補資訊以外的失敗、資料不足、受限報告）保存報告、證據、設定與工具紀錄，含資料版本、模型識別、提示詞版本與用量。
（Pseudocode 42–43；Flowchart FAIL/INSUF/RT→SAVE；DESIGN.md §How to Run）

#### Scenario: Failure run persisted
- **WHEN** 執行因 invalid_plan 失敗
- **THEN** outputs/<run_id>/ 仍包含 tool_log.json、evidence.json 與失敗原因

### Requirement: Bounded context to LLM
系統 SHALL 只把有上限的事件摘要與品質統計交給 LLM，MUST NOT 將完整資料表直接放入提示詞；完整數值保存在 evidence store。
（DESIGN.md §查詢、工具與報告契約補充）

#### Scenario: Large table not sent
- **WHEN** 測試期有 336 個逐時點
- **THEN** 送入 LLM 的內容只含事件摘要與品質統計，不含 336 列原始值

### Requirement: Clear interactive replies
系統 SHALL 以三種明確狀態回覆使用者：完整調查報告、NEED_INPUT（列出缺少欄位與可用範圍）、INSUFFICIENT_DATA／FAILURE（列出原因與已取得證據）。

#### Scenario: Status visible to user
- **WHEN** 執行以 INSUFFICIENT_DATA 結束
- **THEN** 回覆首行顯示狀態與原因，並列出品質摘要，不顯示任何異常結論
