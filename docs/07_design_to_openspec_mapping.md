# 07 DESIGN.md → OpenSpec 對照表

- 左欄來源：[原專案 DESIGN.md（tag before-dic3-20261112）](https://github.com/benson103081/energy-detective-agent/blob/before-dic3-20261112/DESIGN.md)（重構前基準，來源 repo tag `before-dic3-20261112`）。
- 右欄目標：`openspec/specs/<capability>/spec.md`（由 change `convert-design-to-workflow` archive 後產生）。
- 狀態：規格描述「預定系統」；Agent 尚未實作。

## A. 章節／模組對照

| DESIGN.md 章節或模組 | 課程八大模組 | Spec 檔案 | Requirement |
|---|---|---|---|
| §Problem | ① User Scenario | `user-scenario/spec.md` | Single-building anomaly investigation goal |
| §Requirements 第 2 點（歷史重播） | ① | `user-scenario/spec.md` | Historical replay instead of live IoT |
| §Requirements 第 6 點（不控制設備） | ① | `user-scenario/spec.md` | No equipment control |
| Components ① 需求解析與範圍驗證 | ② Query/Input | `query-input/spec.md` | Structured request extraction；Missing building returns NEED_INPUT；Missing period returns NEED_INPUT |
| §查詢、工具與報告契約補充（半開區間、提問視為資料） | ② | `query-input/spec.md` | Half-open period inside registered test window；User input treated as data |
| Components ② 受限規劃與工具控制 | ③ Planner & Reasoning | `planner-reasoning/spec.md` | Shared tool-call budget；Structured plan within whitelist；Single plan repair；Fixed configuration immutable by LLM；Single transient I/O retry；Observation-based next step |
| §AI Model | ③ | `planner-reasoning/spec.md` | Fixed configuration immutable by LLM；Observation-based next step |
| Components ③ CSV 取得與歷史重播 | ④ Data Retrieval | `data-retrieval/spec.md` | Dataset manifest first；Registered data references only；Replay boundary；Raw meter data as primary input |
| Components ④ 時序資料品質檢查；§品質與失敗處理 | ④ | `data-retrieval/spec.md` | Unit and time-axis gate；Coverage threshold gate；No zero-filling and conflict masking；Weather alignment with graceful degradation；Unknown timezone handling |
| §預定資料契約 | ④ | `data-retrieval/spec.md` | Raw meter data as primary input；Unknown timezone handling |
| Components ⑤ 基準與異常分析；§Algorithm 統計基準與異常 | ⑤ Analytics Engine | `analytics-engine/spec.md` | 全部 7 條 |
| Components ⑥ 領域規則與假說；§Domain Knowledge 四條規則 | ⑥ Domain Knowledge | `domain-knowledge/spec.md` | 全部 7 條（四條規則各一條：Historical deviation／Schedule check／Weather context／Zero-run check） |
| Components ⑦ 證據整合與報告（建議與推論邊界部分）；§Limitations | ⑦ Decision/Recommendation | `decision-recommendation/spec.md` | Five-part recommendation structure；No root-cause diagnosis…；Deviation is not verified savings；No-event and no-conclusion outcomes |
| Components ⑦（報告、驗證、模板）；§How to Run 輸出檔 | ⑧ Output & Interaction | `output-interaction/spec.md` | 全部 6 條 |
| Components ⑧ 離線評估；§評估設計 1–6 | （supporting） | `offline-evaluation/spec.md` | 全部 6 條 |
| §說明用合成案例（240 kWh） | ⑤⑦ | `analytics-engine`、`decision-recommendation` | Unit-aware deviation aggregation（Scenario: Illustrative 240 kWh case）；Deviation is not verified savings |

## B. Flowchart 節點對照（DESIGN.md §Data Flow 下方 `flowchart TD`）

| 節點／分支 | 類型 | Spec | Requirement → Scenario |
|---|---|---|---|
| INIT 初始化工具預算 10 次 | 初始化 | planner-reasoning | Shared tool-call budget → Budget exhausted |
| M 檢查資料清單；MV 否 → FAIL | 判斷＋例外 | data-retrieval | Dataset manifest first → Manifest unavailable |
| Q 解析需求 | 處理 | query-input | Structured request extraction → Complete request parsed |
| QV 否 → ASK → END | 判斷＋人機互動 | query-input | Missing building…→ Missing building；Missing period…→ Missing period |
| P 建立受限計畫 | 處理 | planner-reasoning | Structured plan within whitelist → Valid plan executed |
| PV 否 → PR 否 → PF → PV | 迴圈（最多一次） | planner-reasoning | Single plan repair → Repair succeeds |
| PR 是（已修正過）→ FAIL | 例外 | planner-reasoning | Single plan repair → Repair fails；Structured plan… → Unknown tool rejected |
| L 讀取 CSV 與重播時間窗 | 處理 | data-retrieval | Replay boundary → Future data excluded |
| LV 否 → RET 是 → RR → L | 迴圈（重試一次） | planner-reasoning | Single transient I/O retry → Transient error then success |
| RET 否 → FAIL | 例外 | planner-reasoning | Single transient I/O retry → Non-retryable error |
| D 檢查單位、時間軸、缺漏 | 處理 | data-retrieval | Unit and time-axis gate → Unknown unit；No zero-filling… → Conflicting duplicate timestamps |
| DV 否 → INSUF | 判斷＋例外 | data-retrieval | Coverage threshold gate → Insufficient electricity data |
| W 否 → NW | 判斷＋降級 | data-retrieval | Weather alignment with graceful degradation → Weather unavailable |
| A 建立基準、校準、測試 | 處理 | analytics-engine | Chronological split…；Train-only baselines；Calibrated residual threshold；Strict test-period flagging；Same-direction event grouping |
| AV 否 → FAIL | 例外 | analytics-engine | Numeric evidence registration → Analysis failure |
| OBS Agent 依觀察選擇 | AI 決策 | planner-reasoning | Observation-based next step |
| CH 是且驗證通過 → K | 判斷 | domain-knowledge | Precondition-based eligibility → Eligibility computed |
| CH 否且有跳過理由 → R | 判斷 | planner-reasoning | Observation-based next step → No events go straight to report |
| CH 違反前置條件 → FAIL | 例外 | planner-reasoning | Observation-based next step → Ineligible rule selected |
| K 套用規則並建立證據 | 處理 | domain-knowledge | Versioned rule registry；四條規則各自 Scenario |
| R 產生含證據 ID 報告 | 處理 | output-interaction | Evidence-cited report → Report cites evidence |
| RV 否 → RT 固定模板 | 判斷＋降級 | output-interaction | Reference and number validation；Deterministic template fallback → Fallback after validation failure |
| SAVE（含 FAIL、INSUF、RT 匯入） | 保存 | output-interaction | Persist every run → Failure run persisted |
| E 離線評估紀錄 | 評估 | offline-evaluation | 全部 |

## C. Pseudocode 行號對照（DESIGN.md Algorithm 1，01–44）

| 行號 | 內容摘要 | Spec | Requirement |
|---|---|---|---|
| 01 | PROCEDURE INVESTIGATE | user-scenario | Single-building anomaly investigation goal |
| 02 | INIT_GUARD(budget=10, retry, allowed_tools) | planner-reasoning | Shared tool-call budget；Structured plan within whitelist |
| 03 | inspect_dataset | data-retrieval | Dataset manifest first |
| 04 | manifest 失敗 → FAILURE | data-retrieval | Dataset manifest first |
| 05 | PARSE_AND_VALIDATE | query-input | Structured request extraction；Half-open period… |
| 06 | 缺建築／日期 → NEED_INPUT | query-input | Missing building…；Missing period… |
| 07 | LLM_PLAN | planner-reasoning | Structured plan within whitelist；Fixed configuration immutable by LLM |
| 08 | REPAIR_ONCE | planner-reasoning | Single plan repair |
| 09 | 仍無效 → invalid_plan | planner-reasoning | Single plan repair |
| 10 | load_window | data-retrieval | Replay boundary；Registered data references only |
| 11–12 | 暫時性 I/O 重試一次 | planner-reasoning | Single transient I/O retry |
| 13 | 讀取失敗 → FAILURE | planner-reasoning | Single transient I/O retry（Non-retryable error） |
| 14 | check_quality | data-retrieval | Unit and time-axis gate；No zero-filling and conflict masking |
| 15–16 | 單位／時間未定或覆蓋不足 → INSUFFICIENT | data-retrieval | Unit and time-axis gate；Coverage threshold gate |
| 17–18 | analyze_energy | analytics-engine | Numeric evidence registration |
| 19–21 | CHRONOLOGICAL_SPLIT＋ASSERT | analytics-engine | Chronological split with isolation |
| 22–24 | B0／B1 只在訓練期擬合；校準殘差 | analytics-engine | Train-only baselines |
| 25 | QUANTILE(q) | analytics-engine | Calibrated residual threshold |
| 26 | 門檻為 0 → 解析度或失敗 | analytics-engine | Calibrated residual threshold |
| 27–28 | 測試殘差與 flags | analytics-engine | Strict test-period flagging |
| 29 | GROUP_ADJACENT_SAME_DIRECTION | analytics-engine | Same-direction event grouping |
| 30 | REGISTER_EVIDENCE | analytics-engine | Numeric evidence registration |
| 31 | 分析失敗 → FAILURE | analytics-engine | Numeric evidence registration |
| 32 | RULES_ALLOWED_BY | domain-knowledge | Precondition-based eligibility |
| 33 | LLM_NEXT_STEP | planner-reasoning | Observation-based next step |
| 34 | 無效或超預算 → RESTRICTED_REPORT | planner-reasoning | Observation-based next step；Shared tool-call budget |
| 35–36 | apply_domain_rules（eligible 子集） | domain-knowledge | Precondition-based eligibility；Rule tool failure degrades safely |
| 37 | build_report＋RECORD_SKIPPED_RULES | planner-reasoning | Observation-based next step |
| 38 | FREEZE_EVIDENCE_STORE | output-interaction | Evidence-cited report |
| 39 | build_report(require_evidence_ids) | output-interaction | Evidence-cited report；Bounded context to LLM |
| 40 | VALIDATE_REFERENCES_AND_NUMBERS | output-interaction | Reference and number validation |
| 41 | DETERMINISTIC_TEMPLATE | output-interaction | Deterministic template fallback |
| 42 | SAVE(report, evidence, config, log) | output-interaction | Persist every run |
| 43 | RETURN report | output-interaction | Clear interactive replies |
| 44 | END PROCEDURE | — | （結構行，無行為） |

proposal_source.md 內另有 01–37 行的精簡版 Pseudocode（ENERGY_DETECTIVE），對應關係相同：精簡版 10–12 ≈ DESIGN 10–13；13–14 ≈ 14–16；15–25 ≈ 17–30；26–36 ≈ 31–43。

## D. 來源 HEAD 第一版 OpenSpec（12 條）去向

| 來源 HEAD Requirement（[來源 HEAD openspec spec](https://github.com/benson103081/energy-detective-agent/blob/873b14f/openspec/changes/energy-investigation-workflow/specs/energy-investigation/spec.md)） | 本 repo 對應 |
|---|---|
| Request scope | query-input：Missing building returns NEED_INPUT 等 |
| Replay boundary | data-retrieval：Replay boundary |
| Quality gate | data-retrieval：Unit and time-axis gate；Coverage threshold gate |
| Temporal evaluation | analytics-engine：Chronological split…；Train-only baselines |
| Deterministic analytics | analytics-engine：Same-direction event grouping 等 |
| Conditional domain rules | domain-knowledge：Precondition-based eligibility |
| Tool control | planner-reasoning：Shared tool-call budget；Structured plan within whitelist |
| Evidence report | output-interaction：Evidence-cited report；Deterministic template fallback |
| Inference boundary | decision-recommendation：No root-cause diagnosis… |
| Comparable evaluation | offline-evaluation：Three-group fair comparison；No real-fault accuracy claims |
| Report field contract | output-interaction：Reference and number validation；query-input：Half-open period… |
| Registered evaluation cases | offline-evaluation：Fixed agent cases |

## E. 發現的來源不一致（已在 spec 中處理）

1. Flowchart「CH 違反前置條件 → FAIL」vs Pseudocode 34「RESTRICTED_REPORT_WITH_LOG」：spec 採「停止並以已驗證證據產生受限報告＋失敗紀錄」。
2. 來源模組⑧為「離線評估」，課程⑧為「Output & Interaction」：評估獨立為 `offline-evaluation`。
3. DESIGN.md 有 44 行版本、proposal_source.md 有 37 行版本：以 DESIGN.md 為準。
