# offline-evaluation Specification

## Purpose
定義預定的離線評估協議，分別評估統計基準、合成擾動敏感度與 Agent 執行品質，確保比較公平且不以合成或人工標籤冒充真實故障結果。

## Requirements

### Requirement: Baseline comparison on shared points
評估 SHALL 在相同有效測試點上比較 B0 與 B1 的 MAE、覆蓋率、警示點數與事件數；MAE 只代表型態擬合誤差。
（DESIGN.md §評估設計 2；RQ1）

#### Scenario: Shared valid points
- **WHEN** B0 與 B1 的有效點集合不同
- **THEN** 評估只使用兩者交集計算 MAE，並報告有效點數

### Requirement: Synthetic injection on a copy
合成異常 SHALL 注入於測試資料副本，以固定種子（seed = 42）在有效不重疊區間注入尖峰、持續增加與連續零值；基準與門檻 MUST NOT 重訓。
（DESIGN.md §評估設計 3）

#### Scenario: Original test set untouched
- **WHEN** 完成合成注入實驗
- **THEN** 原始測試集檔案雜湊值不變，基準與門檻與注入前相同

### Requirement: Fixed agent cases
評估 SHALL 使用預先登錄的 C01–C10 案例，每組方法每案例重跑 3 次；預期失敗案例只有正確停止才算成功。
（DESIGN.md §評估設計 4；evaluation_protocol.md）

#### Scenario: Expected failure scored correctly
- **WHEN** C04（缺建築）的執行仍產生異常報告
- **THEN** 該次執行記為失敗

### Requirement: Three-group fair comparison
評估 SHALL 以相同問題、資料、工具與 B1 結果比較固定 Python 報表、固定流程＋LLM 與受限 Agent，MUST NOT 預設 Agent 較佳或把 B0／B1 差異歸因於 Agent。
（DESIGN.md §評估設計 6；RQ2）

#### Scenario: Agent not better
- **WHEN** 固定流程＋LLM 與 Agent 完整度相同且成本較低
- **THEN** 結果照實報告並建議固定流程，不更換指標

### Requirement: No real-fault accuracy claims
在沒有現場確認根因標籤時，評估 MUST NOT 報告真實設備診斷 precision／recall；合成結果只代表指定擾動的敏感度。
（DESIGN.md §評估設計 3、5）

#### Scenario: No real fault labels
- **WHEN** 資料沒有現場確認的根因標籤
- **THEN** 報告只呈現注入事件命中率與人工審閱結果，並註明不代表真實故障偵測率

### Requirement: Traceability metric definition
事實可追溯率 SHALL 定義為「有有效證據且數值／單位相符的事實陳述數 ÷ 全部事實陳述數」；無事實陳述時 SHALL 記為 N/A 而非 100%。
（DESIGN.md §評估設計 5）

#### Scenario: Report without facts
- **WHEN** 報告為 NEED_INPUT 回覆且不含事實陳述
- **THEN** 該次可追溯率記為 N/A
