# planner-reasoning Specification

## Purpose
定義受限規劃 Agent 的行為：LLM 只能在工具白名單、依賴順序與呼叫預算內產生結構化計畫並實際執行，於分析後依觀察做一次可驗證的下一步決策。

## Requirements

### Requirement: Shared tool-call budget
系統 SHALL 在第一次工具呼叫前初始化共用預算：每次調查最多 10 次工具呼叫（含 inspect_dataset 與重試）；達上限即停止並保留已取得證據與紀錄。
（Pseudocode 02；Flowchart 節點 INIT；DESIGN.md §Data Flow 預算說明）

#### Scenario: Budget exhausted
- **WHEN** 第 10 次工具呼叫完成後計畫仍要求下一個工具
- **THEN** 系統不再呼叫工具，輸出帶執行紀錄的失敗或受限報告，工具呼叫總數為 10

### Requirement: Structured plan within whitelist
系統 SHALL 由 LLM 依需求、資料清單與固定設定產生結構化計畫（goal、building_id、日期、方向、工具、參數、候選規則），並由程式驗證 schema、白名單、依賴順序、資料範圍與預算。
（Pseudocode 07；Flowchart 節點 P、PV）

#### Scenario: Valid plan executed
- **WHEN** 計畫依序包含 load_window、check_quality、analyze_energy 且參數皆在範圍內
- **THEN** 控制器以計畫中的工具名與參數實際呼叫工具，而非只輸出計畫文字

#### Scenario: Unknown tool rejected
- **WHEN** 計畫包含未登錄工具、任意 Shell、程式碼、URL 或 SQL
- **THEN** 驗證器拒絕該計畫並保存驗證錯誤，不執行任何被拒絕的工具或參數

### Requirement: Single plan repair
系統 SHALL 在計畫驗證失敗時僅依驗證錯誤修正一次；修正後仍無效 SHALL 回傳 `invalid_plan` 失敗並保存紀錄。
（Pseudocode 08–09；Flowchart PV→PR→PF→PV、PR→FAIL；評估案例 C09）

#### Scenario: Repair succeeds
- **WHEN** 第一次計畫的期間越界，修正後計畫通過驗證
- **THEN** 系統執行修正後計畫，紀錄中保留原始錯誤與一次修正

#### Scenario: Repair fails
- **WHEN** 修正一次後計畫仍包含未知工具
- **THEN** 系統回傳 FAILURE（invalid_plan）並保存紀錄，不進行第二次修正

### Requirement: Fixed configuration immutable by LLM
LLM MUST NOT 變更時間切分、品質門檻、異常門檻或分位數設定；計畫只允許選擇查詢範圍、關注方向與候選規則。
（DESIGN.md §工具輸入與輸出、§AI Model）

#### Scenario: Plan modifies threshold
- **WHEN** 計畫參數嘗試把異常分位數從 0.99 改為 0.9
- **THEN** 驗證器視為越界參數並拒絕該計畫

### Requirement: Single transient I/O retry
系統 SHALL 僅在資料讀取發生暫時性 I/O 錯誤且尚未重試時，以相同參數重試一次；欄位缺漏、檔案不存在、品質不足或演算法錯誤 MUST NOT 重試。
（Pseudocode 10–13；Flowchart L→LV→RET→RR→L、RET→FAIL；評估案例 C08）

#### Scenario: Transient error then success
- **WHEN** 第一次 load_window 回傳 retryable 的暫時性 I/O 錯誤
- **THEN** 系統以相同參數重試一次，紀錄兩次呼叫且均計入 10 次預算

#### Scenario: Non-retryable error
- **WHEN** load_window 回傳檔案不存在（retryable = false）
- **THEN** 系統不重試，直接輸出失敗原因與現有證據

### Requirement: Observation-based next step
系統 SHALL 在分析完成後讓 LLM 依品質與事件觀察做一次決策：呼叫 apply_domain_rules（規則須為前提允許的 eligible 子集）或直接 build_report 並記錄跳過理由。
（Pseudocode 32–37；Flowchart OBS→CH→K 或 CH→R）

#### Scenario: No events go straight to report
- **WHEN** 分析結果沒有任何候選事件
- **THEN** Agent 選擇 build_report，並記錄「無事件，略過規則」作為跳過理由

#### Scenario: Ineligible rule selected
- **WHEN** 天氣無法對齊但 Agent 選擇 weather_context
- **THEN** 控制器拒絕該決策並停止，只以已驗證證據產生受限報告與失敗紀錄
