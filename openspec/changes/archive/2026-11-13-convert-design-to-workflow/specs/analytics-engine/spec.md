# Spec Delta：⑤ Analytics Engine

## Purpose
定義由 Python 確定性計算的統計基準、校準門檻、測試期殘差與事件合併行為，確保時間隔離、可重現，且 LLM 不產生任何數值。

## ADDED Requirements

### Requirement: Chronological split with isolation
系統 SHALL 依固定日期把有效資料切為訓練（暫定 8 週）、校準（2 週）、測試（2 週），並斷言三段依序不重疊且測試期不晚於 replay_until。
（Pseudocode 19–21）

#### Scenario: Overlapping split rejected
- **WHEN** 設定的校準期起點早於訓練期終點
- **THEN** analyze_energy 斷言失敗並回傳 error，不輸出任何事件

### Requirement: Train-only baselines
系統 SHALL 以訓練期擬合 B0（全域中位數）與 B1（同星期×小時中位數），MUST NOT 使用校準或測試資料擬合基準；報告以 B1 為主並保留 B0 比較。
（Pseudocode 22–24）

#### Scenario: Test data isolated
- **WHEN** 修改保留測試期的觀測值後重跑
- **THEN** B0、B1 參數與門檻保持不變，只有測試期事件可能改變

### Requirement: Calibrated residual threshold
系統 SHALL 以校準期絕對殘差的固定分位數（暫定 0.99、linear 插值）作為門檻；門檻為 0 時 SHALL 改用已核對的電表解析度，解析度未知則回傳不可校準。
（Pseudocode 25–26）

#### Scenario: Zero threshold without resolution
- **WHEN** 校準期殘差全為 0 且電表解析度無法由來源確認
- **THEN** 系統回傳 not_calibratable 錯誤，不臨時猜測門檻

### Requirement: Strict test-period flagging
系統 SHALL 只在測試期以 `abs(residual) > threshold`（嚴格大於）標記候選異常，並保留方向、偏差量與觀測值。
（Pseudocode 27–28）

#### Scenario: Residual equal to threshold
- **WHEN** 某測試點殘差絕對值恰等於門檻
- **THEN** 該點不被標記為候選異常

### Requirement: Same-direction event grouping
系統 SHALL 將相鄰且同方向的警示點合併為事件；遇缺測、非警示點或方向變化即中斷；單點保留為單點事件。
（Pseudocode 29）

#### Scenario: Event merging
- **WHEN** 連續四個有效小時皆高於基準且超過門檻
- **THEN** 合併為一個四小時偏高事件

#### Scenario: Missing value breaks event
- **WHEN** 四小時偏高序列的第三小時缺測
- **THEN** 系統輸出兩個事件，不補零也不跨缺口合併

### Requirement: Unit-aware deviation aggregation
系統 SHALL 僅在確認為固定間隔電能時直接加總偏差；功率須依已驗證取樣間隔積分；偏差量 MUST 標示為「相對基準的估計差異」，不得稱為節電量。
（DESIGN.md §統計基準與異常）

#### Scenario: Illustrative 240 kWh case
- **WHEN** 四小時各觀測 180 kWh、B1 各 120 kWh、門檻 30 kWh
- **THEN** 事件偏離總和為 240 kWh，並標示為 deviation 而非 savings

### Requirement: Numeric evidence registration
系統 SHALL 為每個方法登錄證據：方法、門檻、事件、品質、資料版本與原始資料定位；分析失敗 SHALL 回傳帶紀錄的失敗。
（Pseudocode 30–31；Flowchart A→AV、AV→FAIL）

#### Scenario: Analysis failure
- **WHEN** analyze_energy 拋出演算法錯誤
- **THEN** 系統不重試，保存現有品質證據與錯誤碼並回傳 FAILURE_WITH_LOG
