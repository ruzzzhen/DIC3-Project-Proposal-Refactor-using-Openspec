# user-scenario Specification

## Purpose
定義 Energy Detective 服務的使用者、調查目標與應用邊界：設施管理者以歷史重播資料調查單棟建築的用電異常，取得待查假說與查核建議，而非設備故障確診或節電宣稱。

## Requirements

### Requirement: Single-building anomaly investigation goal
系統 SHALL 以「調查指定建築在指定期間內值得追查的用電異常」作為唯一支援的任務目標，輸出含時間、數值、證據與後續查核建議的調查結果。
（來源：DESIGN.md §Problem、§Requirements 第 3 點）

#### Scenario: Facility manager asks for anomalies
- **WHEN** 設施管理者提問「請調查建築 X 在測試期間有哪些值得調查的用電異常」
- **THEN** 系統啟動異常調查流程，最終輸出含事件時間、偏差數值、證據 ID 與下一步查核建議的報告

#### Scenario: Unsupported task type
- **WHEN** 使用者要求預測下個月電費或比較兩個城市的電價
- **THEN** 系統回覆此任務不在支援範圍，說明可支援的調查目標，不呼叫分析工具

### Requirement: Historical replay instead of live IoT
系統 SHALL 以公開歷史電表 CSV 重播模擬 IoT 資料流，MUST NOT 宣稱已完成即時感測器串接或現場部署。
（來源：DESIGN.md §Requirements 第 2 點、§Data Flow）

#### Scenario: Replay disclosed in report
- **WHEN** 系統產生任何調查報告
- **THEN** 報告註明資料來源為 BDG2 歷史資料與重播截止點，不描述為即時監測結果

### Requirement: No equipment control
系統 MUST NOT 執行任何設備控制、排程變更或寫入建築系統的動作；只能提供查核建議。
（來源：DESIGN.md §Requirements 第 6 點）

#### Scenario: User asks to switch off equipment
- **WHEN** 使用者要求「直接把夜間空調關掉」
- **THEN** 系統拒絕執行控制動作，僅回覆可查核的紀錄與建議由人員確認
