# References（引用與查證紀錄）

- **查證日期：** 全部為 **2026-11-13**（線上開啟原始頁面、arXiv 摘要頁、Crossref／DataCite 書目 API，或官方開放資料）。
- **編號規則：** `A*` = AI Technology；`D*` = Domain Know-how。A1–A5、D1–D9 沿用來源專案編號（[原專案 research/proposal_source.md（tag before-dic3-20261112）](https://github.com/benson103081/energy-detective-agent/blob/before-dic3-20261112/research/proposal_source.md) §6），A6 起、D10 起為本次新增。
- **狀態：** ✅ 已查證｜✏️ 已查證並修正來源專案的寫法｜⚠️ 部分查證（見備註）。
- 只有 `[UNVERIFIED]` 區的項目沒有找到原始來源。正文**沒有**使用任何未查證的數字。

## AI Technology

| ID | 作者 | 年份 | 標題 | 出處 | DOI／URL | 狀態與查證重點 |
|---|---|---|---|---|---|---|
| A1 | Shunyu Yao, Jeffrey Zhao, Dian Yu, Nan Du, Izhak Shafran, Karthik Narasimhan, Yuan Cao | 2023 | ReAct: Synergizing Reasoning and Acting in Language Models | ICLR 2023 | https://arxiv.org/abs/2210.03629 | ✅ arXiv v3 註明為 ICLR camera-ready 版本 |
| A2 | Yile Gu, Yifan Xiong, Jonathan Mace, Yuting Jiang, Yigong Hu, Baris Kasikci, Peng Cheng | 2025 | Argos: Agentic Time-Series Anomaly Detection with Autonomous Rule Generation via Large Language Models | arXiv preprint | https://doi.org/10.48550/arXiv.2501.14170 | ✏️ 補上完整作者。目前只有 v1，未找到正式發表處，引用為 preprint |
| A3 | Alan Yang, Yulin Chen, Sean Lee, Venus Montes | 2025 | Refining Time Series Anomaly Detectors using Large Language Models | arXiv preprint | https://arxiv.org/abs/2503.21833 | ✅ 作者正確 |
| A4 | Ozan Baris Mulayim, Anand Krishnan Prakash, Lazlo Paul, Marco Pritoni | 2025 | Extraction and Analysis of Time Series Data from Building Automation Systems Using Large Language Models | 2025 ASHRAE Annual Conference（LBNL 書目） | https://doi.org/10.20357/B73W4C | ✏️ DataCite 確認標題、作者與年份。Table 1：三題各問五次，氣流下降題 3/5 ✅。會議名稱取自 LBNL 書目，全文 PDF 未標示會議名 |
| A5 | Yiyuan Yang, Zichuan Liu, Lei Song, Kai Ying, Zhiguang Wang, Tom Bamford, Svitlana Vyetrenko, Jiang Bian, Qingsong Wen | 2025（v5 2026） | Time-RA: Towards Time Series Reasoning for Anomaly **Diagnosis** with LLM Feedback | Findings of ACL 2026 | https://arxiv.org/abs/2507.15066 | ✏️ **來源專案標題漏了 "Diagnosis"**，已修正。arXiv Comments 原文：「ACL 2026 Findings」。RATs40K 約 4 萬筆、10 個領域；使用 LoRA ✅ |
| A6 | Zihao Zhou, Rose Yu | 2025 | Can LLMs Understand Time Series Anomalies? | ICLR 2025 | https://arxiv.org/abs/2410.05440 ；https://proceedings.iclr.cc/paper_files/paper/2025/file/05774fb74e863308c4b460c9f49f6918-Paper-Conference.pdf | ✅ 摘要寫明：明確推理提示不能改善表現、影像輸入優於文字、「no evidence that they can understand more subtle real-world anomalies」 |
| A7 | Tianzhi He, Farrokh Jazizadeh | 2025 | Context-aware LLM-based AI Agents for Human-centered Energy Management Systems in Smart Buildings | arXiv preprint | https://arxiv.org/abs/2512.25055 | ✅ 120 個查詢、4 個住宅資料集。正確率：裝置控制 86%、記憶 97%、排程 74%、能源分析 77%、成本估算 49% |
| A8 | Ozan Baris Mulayim, Pengrui Quan, Liying Han, Xiaomin Ouyang, Dezhi Hong, Mario Bergés, Mani Srivastava | 2025 | Can Time-Series Foundation Models Perform Building Energy Management Tasks? | arXiv preprint（under review, Data-Centric Engineering） | https://arxiv.org/abs/2506.11250 | ✅ 摘要原文：TSFM「only marginally better than statistical models on unseen datasets」 |
| A9 | Jian Zhang, Chaobo Zhang, Jie Lu, Yang Zhao | 2025 | Domain-specific large language models for fault diagnosis of heating, ventilation, and air conditioning systems by labeled-data-supervised fine-tuning | Applied Energy, 377 | https://doi.org/10.1016/j.apenergy.2024.124378 | ✅ Crossref 確認書目資料。正確率由 29.5% 提升到 100%、比 GPT-4 平均高 31.1%：取自研究代理查閱的摘要，本人未開啟全文（正文只寫「明顯優於」，未引用這兩個數字） |
| A10 | Alexander Neubauer, Tianzhen Hong, Han Li, Mengbo Yu, Amin Darbandi, Yannick Fürst, Martin Kriegel | 2026 | Large Language Models for HVAC Operations in Building Energy Systems: A Critical Review of Methods, Applications, and Deployment Readiness | arXiv preprint（submitted to Energy and Buildings） | https://arxiv.org/abs/2609.05314 | ✅ 編碼 66 篇研究（2023–2026/3）。「Only four studies reach pilot-level evidence, and none reports sustained operational deployment」 |

## Domain Know-how

| ID | 作者 | 年份 | 標題 | 出處 | DOI／URL | 狀態與查證重點 |
|---|---|---|---|---|---|---|
| D1 | International Energy Agency | 2026 | Global Energy Review 2026 — Electricity demand | IEA | https://www.iea.org/reports/global-energy-review-2026/electricity-demand | ✅ 2025 年全球用電成長約 3%；建築占全年**增量**近 45%（不是占總量 45%） |
| D2 | International Energy Agency | 2026 | Electricity 2026 — Demand | IEA | https://www.iea.org/reports/electricity-2026/demand | ✅ 預測建築（住宅＋商業）占 2025–2030 年新增需求 49%（這是**預測**） |
| D3 | Clayton Miller, Anjukan Kathirgamanathan, Bianca Picchetti, Pandarasamy Arjunan, June Young Park, Zoltan Nagy, Paul Raftery, Brodie W. Hobson, Zixiao Shi, Forrest Meggers | 2020 | The Building Data Genome Project 2, energy meter data from the ASHRAE Great Energy Predictor III competition | Scientific Data, 7, 368 | https://doi.org/10.1038/s41597-020-00712-x | ✏️ 補上完整標題與作者。19 個場址、1,636 棟非住宅建築、3,053 個計量表、2016–2017 逐時資料 ✅ |
| D4 | BUDS Lab | n.d. | building-data-genome-project-2（資料 repository） | GitHub | https://github.com/buds-lab/building-data-genome-project-2 | ✅ |
| D5 | BUDS Lab | n.d. | LICENSE | GitHub | https://raw.githubusercontent.com/buds-lab/building-data-genome-project-2/master/LICENSE | ✅ 檔頭原文為「Attribution-ShareAlike 4.0 Unported」（非標準寫法，實質是 CC BY-SA 4.0） |
| D6 | BUDS Lab | n.d. | Meters data features（wiki） | GitHub Wiki | https://github.com/buds-lab/building-data-genome-project-2/wiki/Meters-data-features | ✏️ cleaned 移除 S-H-ESD 離群值、**連續超過 24 小時**的零值，以及電表的絕對零值。逐時、當地時區。注意：部分 Kaggle 衍生資料未完全換算為 kWh |
| D7 | Hannah Kramer, Claire Curtin, Guanjing Lin, Eliot Crowe, Jessica Granderson（依 LBNL 頁面） | 2020 | Proving the Business Case for Building Analytics | Lawrence Berkeley National Laboratory | https://doi.org/10.20357/B7G022 ；https://bies.lbl.gov/publications/proving-business-case-building | ⚠️ 104 個組織、約 6,500 棟建築；EIS 與 FDD 年節能中位數 3% 與 9% ✅。DataCite 的作者列表與 LBNL 頁面不同，本表採 LBNL 頁面；報告編號未找到 |
| D8 | U.S. DOE, Federal Energy Management Program | 頁面最後更新 2023-04-18 | Measurement and Verification Options for Federal Energy- and Water-Saving Projects | energy.gov | https://www.energy.gov/femp/measurement-and-verification-options-federal-energy-and-water-saving-projects | ✏️ 補上更新日期。Option A–D ✅；「Option C cannot verify the performance of individual measures」✅ |
| D9 | Young Jin Park, Francois Germain, Jing Liu, Ye Wang, Toshiaki Koike-Akino, Gordon Wichern, Navid Azizan, Christopher R. Laughman, Ankush Chakrabarty | 2025 | Probabilistic Forecasting for Building Energy Systems using Time-Series Foundation Models | arXiv preprint（初版見 NeurIPS TSALM Workshop） | https://arxiv.org/abs/2506.00630 | ✏️ 補上完整作者。Chronos 見 §3.3，SUSTIE 見 §1、§3.1，零樣本／全量微調／LoRA 見 §4.1–4.3（摘要本身未提及 Chronos 與 SUSTIE） |
| D10 | Manoj Gulati, Pandarasamy Arjunan | 2022 | LEAD1.0: A Large-scale Annotated Dataset for Energy Anomaly Detection in Commercial Buildings | arXiv preprint | https://arxiv.org/abs/2203.17256 | ✅ 1,413 條電表時序、超過一年、取自 ASHRAE GEPIII（與 BDG2 同源）；評測 8 種方法。正式會議出處未查到，引用為 preprint |
| D11 | Jessica Granderson, Samir Touzani, Claudine Custodio, Michael D. Sohn, David Jump, Samuel Fernandes | 2016 | Accuracy of automated measurement and verification (M&V) techniques for energy savings in commercial buildings | Applied Energy, 173, 296–308 | https://doi.org/10.1016/j.apenergy.2016.04.049 | ✅ Crossref 確認書目資料。537 棟建築、10 種基準模型；12 個月訓練資料下中位數 CV(RMSE) < 25%（OSTI／LBNL 摘要） |
| D12 | Zhelun Chen, Zheng O'Neill, Jin Wen, Ojas Pradhan, Tao Yang, Xing Lu, et al. | 2023 | A review of data-driven fault detection and diagnostics for building HVAC systems | Applied Energy, 339, 121030 | https://doi.org/10.1016/j.apenergy.2023.121030 | ✅ Crossref 確認書目資料。內容重點（FDD 流程、部署與可解釋性挑戰）取自研究代理查閱的摘要 |
| D13 | Fahimeh Orvati Nia, Shima Salehi, Joshua Peeples | 2026 | Evaluating GAN-LSTM for Smart Meter Anomaly Detection in Power Systems | 2026 IEEE Texas Power and Energy Conference (TPEC) | https://doi.org/10.1109/TPEC67884.2026.11513138 ；https://arxiv.org/abs/2601.09701 | ✅ Crossref 確認書目資料；arXiv 摘要確認：LEAD、406 棟建築一年逐時、6 個 baseline、GAN-LSTM F1 = 0.89 |
| D14 | International Energy Agency | 2026 | Special Report on Buildings Efficiency — Executive summary | IEA | https://www.iea.org/reports/special-report-on-buildings-efficiency/executive-summary | ✅ 原文：建築用能「accounts for around 30% of total final consumption, half of global electricity consumption」 |
| D15 | 經濟部能源署 | 2026（資料至 2026/09） | 電力消費月資料 | 政府資料開放平臺 | https://data.gov.tw/dataset/161209 | ✅ 下載官方 CSV 並自行加總 12 個月：2025 年總計 283,008.6 百萬度，服務業部門 49,908.6（17.6%）、住宅部門 53,737.4（19.0%），合計 36.6%；2024 年分別為 17.6% 與 18.8%。**比例是本專題自行計算**，近期月份可能修正 |

## 本次刻意不採用的項目

| 項目 | 原因 |
|---|---|
| 來源 D7 的 2019 年 DOE 新聞稿（96 個組織、EIS 4%） | 和 2020 年 LBNL 報告（104 個組織、3%）屬同一計畫的不同階段，兩組數字容易混用。正文只採 2020 報告 |
| ASHRAE Guideline 14-2023 的 CV(RMSE)／NMBE 門檻 | 無法取得原文核對，正文不引用門檻數值 |
| AnomaMind（arXiv:2602.13807）、PILLM（arXiv:2510.17146）、TSFM 負載預測基準（arXiv:2602.10848）、智慧電表補值（arXiv:2608.21638） | 研究代理找到了，但本人未逐篇核對。為控制查證範圍，不納入正文 |

## [UNVERIFIED]

以下項目找不到原始來源或無法完整核對。正文不使用這些數字，或已明示限制。

1. **[UNVERIFIED] D7 的 LBNL 報告編號：** 頁面與 DataCite 都沒有顯示報告編號，DataCite 的作者列表也和 LBNL 頁面不一致。請人工到 LBNL 頁面確認作者順序。
2. **[UNVERIFIED] 「商業建築浪費 5–30% 能源」之類的常見說法：** 無法追到原始出處，**已從所有文件排除**。
3. **[UNVERIFIED] A4 的會議名稱（2025 ASHRAE Annual Conference）：** 來自 LBNL 書目的搜尋結果，全文 PDF 沒有寫會議名。請人工確認。
4. **[UNVERIFIED] D10 LEAD1.0 的正式發表處：** arXiv 頁面沒有列出 venue，所以只以 preprint 引用。
