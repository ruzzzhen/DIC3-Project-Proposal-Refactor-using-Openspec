# data-retrieval Specification

## Purpose
定義從已登錄的 BDG2 本機資料取得電表與天氣時序、限制重播可見時間、保存來源定位，並在分析前執行資料品質閘門的行為。

## Requirements

### Requirement: Dataset manifest first
系統 SHALL 先以 inspect_dataset 取得資料清單（可用建築、電表種類、期間、單位／時區來源、檔案版本）；清單取得失敗 SHALL 回傳帶紀錄的失敗。
（Pseudocode 03–04；Flowchart M→MV、MV→FAIL）

#### Scenario: Manifest unavailable
- **WHEN** 本機資料 ID 未登錄或清單檔讀取失敗
- **THEN** 系統回傳 FAILURE_WITH_LOG 並結束，不進行需求解析後的任何分析

### Requirement: Registered data references only
資料工具 SHALL 只接受已登錄的本機資料 ID 與表格參照，MUST 拒絕任意檔案路徑、URL 與 SQL。
（DESIGN.md §工具輸入與輸出）

#### Scenario: Arbitrary path rejected
- **WHEN** load_window 收到 `../../etc/passwd` 或外部 URL 作為資料來源
- **THEN** 工具回傳 status = error 與 error_code，不讀取該路徑

### Requirement: Replay boundary
系統 SHALL 只提供重播截止點 `replay_until` 以前的資料，並記錄資料版本、原始檔名與列／欄定位。
（Pseudocode 10、21；DESIGN.md 模組③）

#### Scenario: Future data excluded
- **WHEN** replay_until 早於資料檔最後一筆時間
- **THEN** 回傳資料表中所有時間戳均早於或等於 replay_until

### Requirement: Raw meter data as primary input
系統 SHALL 以 BDG2 raw electricity 作為主要分析輸入，並將寬表轉為含 timestamp、building_id、meter_id、value、unit 的長表；cleaned 版本只能作為另行標示的敏感度分析。
（DESIGN.md §Data Flow、§預定資料契約）

#### Scenario: Cleaned data requested as main input
- **WHEN** 設定檔將 cleaned 指定為主測試資料
- **THEN** 系統拒絕以 cleaned 取代主測試，只允許標記為 sensitivity 的額外執行

### Requirement: Unit and time-axis gate
系統 SHALL 在分析前確認量測單位與時間標記語意；單位或時間軸無法確認時 MUST 停止數值分析並回傳品質報告。
（Pseudocode 14–16；Flowchart D→DV→INSUF）

#### Scenario: Unknown unit
- **WHEN** 電表欄位單位無法確認為逐時 kWh 或已驗證的功率
- **THEN** 系統回傳 insufficient_data 與品質證據，不混用功率、電能與累積讀數

### Requirement: Coverage threshold gate
系統 SHALL 以完整時間格點計算有效率；任一分割有效率低於固定設定（暫定 90%）或訓練期任一星期×小時組別少於 4 個有效點時，SHALL 回傳資料不足報告。
（Pseudocode 15–16；Flowchart DV→INSUF；評估案例 C06）

#### Scenario: Insufficient electricity data
- **WHEN** 測試期有效率為 85%
- **THEN** 系統回傳 INSUFFICIENT_DATA 與品質證據，不產生異常或節電結論

### Requirement: No zero-filling and conflict masking
系統 MUST NOT 對缺失量測補零或以測試期插補；相同時間相同值可去重並記錄，相同時間不同值 SHALL 標記衝突並排除；負值 SHALL 先列為品質疑點。
（DESIGN.md §品質與失敗處理）

#### Scenario: Conflicting duplicate timestamps
- **WHEN** 同一建築同一時間出現 100 kWh 與 140 kWh 兩筆值
- **THEN** 兩筆皆被遮罩為衝突並排除分析，品質摘要記錄衝突數，不取平均

### Requirement: Weather alignment with graceful degradation
系統 SHALL 只以同場址、同時刻對齊天氣；天氣無法對齊時 SHALL 保留用電分析、記錄限制並停用天氣相關規則。
（Flowchart W→NW→A；評估案例 C07）

#### Scenario: Weather unavailable
- **WHEN** 電量資料合格但該場址天氣缺漏
- **THEN** 系統繼續基準與異常分析，weather_context 標為不可用並在報告中說明

### Requirement: Unknown timezone handling
資料未提供時區時，系統 SHALL 保留來源原始時間軸並標示時區未知，MUST NOT 自行假設為 UTC，且 SHALL 停用需要可靠當地時間的作息規則。
（DESIGN.md §預定資料契約）

#### Scenario: Timezone missing
- **WHEN** metadata 無法確認建築時區
- **THEN** 分析使用原始時間軸，schedule_check 被列為不適用並記錄原因
