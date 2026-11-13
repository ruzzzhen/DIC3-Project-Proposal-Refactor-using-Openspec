# Spec Delta：⑦ Decision / Recommendation

## Purpose
定義 Agent 如何把數值證據與規則結果整合為可行動但不過度推論的調查建議，明確區分觀察事實、待查假說、尚缺證據與下一步查核。

## ADDED Requirements

### Requirement: Five-part recommendation structure
每則建議 SHALL 依序包含：觀察事實 → 比較基準 → 可能解釋（假說）→ 尚缺證據 → 下一步查核，並附資料期間、證據 ID 與規則來源。
（Proposal Introduction K. Keypoint；DESIGN.md §Domain Knowledge）

#### Scenario: Recommendation for night load
- **WHEN** 報告包含一個夜間偏高事件
- **THEN** 該建議五個部分皆存在，且每個數值都附 evidence_id

### Requirement: No root-cause diagnosis from whole-building meter
系統 MUST NOT 僅憑建築總表宣告特定設備故障；缺少設備狀態或維修紀錄時 SHALL 列出替代解釋與區辨所需證據。
（DESIGN.md §Limitations；評估案例 C10）

#### Scenario: User demands exact faulty AC
- **WHEN** 使用者要求「指出哪台空調故障」
- **THEN** 系統說明總表證據不足以確認設備，提供可行的調查步驟與所需補充資料

### Requirement: Deviation is not verified savings
系統 MUST NOT 將偏離基準的電量稱為浪費量、節電量或投資回收；只能稱為「相對基準的偏離電量」。
（DESIGN.md §統計基準與異常；DOE FEMP M&V 邊界）

#### Scenario: Savings claim blocked
- **WHEN** 報告草稿出現「已節省 240 kWh」
- **THEN** 驗證將其計為未支持根因／節電斷言，最終報告改寫為偏離電量或改用模板

### Requirement: No-event and no-conclusion outcomes
沒有事件時系統 SHALL 回報「未發現超過本門檻的事件」而 MUST NOT 宣稱沒有故障；資料不足時 SHALL 允許不給異常結論。
（評估案例 C03、C06）

#### Scenario: No alerts
- **WHEN** 測試期無任何點超過門檻
- **THEN** 報告寫明「在目前門檻下未發現事件」並附門檻與覆蓋率，不寫「建築運轉正常」
