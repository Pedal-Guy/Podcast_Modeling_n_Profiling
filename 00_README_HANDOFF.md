# Modeling and Profiling — 本集工作目錄交接說明（HANDOFF）

> 給下一個「零上下文」Session：先讀這份檔案，就能知道本集做到哪裡、檔案在哪、下一步是什麼。

## 本集目標
- Podcast 長度：1.5–2 小時，主題為吉他音箱數位模擬的兩大技術：**Modeling** 與 **Profiling / Capture**。
- 主持人：Emil（Positive Grid Web & IT Service 總監，吉他/錄音愛好者），使用繁體中文。
- 使用者要求的產出：所有參考資料連結、時間架構與脈絡、Podcast 講稿（提詞稿）、技術名詞簡易說明、技術圖表。
- 使用者要求：**所有產出都要存成檔案放在本資料夾**，讓新 Session 只讀檔案就能繼續工作。
- 資料夾結構：Podcast/ 底下每個主題一個 subfolder，本集為 `Modeling and Profiling/`。

## 研究日期
2026-09-27（研究以此日期為準；A2、Helix Stadium Proxy、Kemper Profiling 2.0、TONE3000 Plugin 等都是 2025–2026 的新資訊）

## Git 遠端
- GitHub：https://github.com/Pedal-Guy/Podcast_Modeling_n_Profiling（branch `main`）
- 本資料夾內容即為該 repo 的根目錄；repo 的 README.md 是對外的簡介，本檔是詳細交接說明。

## 線上文件（Claude Docs）
- 同步版本的研究與講稿文件：https://claude.ai/code/artifact/954dabe4-3cd8-4abb-91ea-edf11b82d209
- 本資料夾的 Markdown 檔是「主要真實來源」（source of truth）；Docs 是閱讀/分享版。

## 檔案地圖
| 檔案 | 內容 |
|---|---|
| 00_README_HANDOFF.md | 本檔：狀態、檔案地圖、下一步 |
| 01_research_raw/A_modeling_history.md | 原始研究（英文）：Modeling 歷史、產品時間表、社群觀點 |
| 01_research_raw/B_profiling_capture_market_hybrid.md | 原始研究（英文）：Kemper、Neural DSP、TONEX、Proxy、市場比較、混合方案 |
| 01_research_raw/C_ir_and_academic_papers.md | 原始研究（英文）：IR/卷積、白/灰/黑盒、論文清單、盲測證據 |
| 01_research_raw/D_nam_a2_tone3000.md | 原始研究（英文）：NAM、A2 Full/Lite、NAM 硬體、TONE3000 |
| 02_core_answers.md | 問題 1–5 的中文整理答案 |
| 03_timeline.md | 時間軸與歷史脈絡 |
| 04_tech_deep_dive.md | 技術深入（白盒/灰盒/黑盒、IR、神經網路、論文） |
| 05_nam_a2.md | NAM 與 A2 |
| 06_market_comparison.md | 市場產品比較 |
| 07_tone3000.md | TONE3000 與檔案格式 |
| 08_hybrid_idea.md | 「Modeling 為底、Profiling 修正」可行性分析 |
| 09_episode_rundown.md | 節目架構與時間分配 |
| 10_script.md | Podcast 提詞稿 |
| 11_glossary.md | 技術名詞表 |
| 12_references.md | 參考資料連結總表 |
| 13_bias_amp_match.md | 補充：Positive Grid BIAS Amp Match 的歷史、運作、限制與節目用法 |
| 14_nam_origin_training_a2.md | 補充：NAM 源頭細節、訓練（修正）流程與參數、A1 vs A2 架構對照、A2 解決的問題與開發過程 |
| 15_fractal_tone_match_and_differentiable_whitebox.md | 補充：Fractal Tone Match 與可微分白盒研究的做法、案例、限制，以及三種「以模型為底」做法的比較 |
| diagrams/ | 5 張 PNG 圖表 + README_diagrams.md（每張圖的用途與 Mermaid 原始碼） |

## 狀態
（見檔案底部「進度紀錄」，每完成一步就更新）

## 進度紀錄
- [x] 2026-09-27 研究完成（4 路並行：Modeling 歷史／Profiling 與市場／IR 與論文／NAM-A2-TONE3000），原始報告存於 01_research_raw/
- [x] 2026-09-27 中文整理檔 02–12 完成（答案、時間軸、技術、NAM/A2、市場、TONE3000、混合想法、節目架構、提詞稿、名詞表、參考資料）
- [x] 2026-09-27 圖表 diagrams/ 完成：5 張 PNG（01_signal_chain、02_five_eras、03_three_approaches、04_nam_workflow、05_hybrid_architecture）＋ README_diagrams.md（含 Mermaid 原始碼，可重繪）
- [x] 2026-09-27 Claude Docs 文件 12 個章節全部同步（內容與 02–12 檔相同；圖表在文件中是可編輯的互動圖）
- [x] 2026-09-27 應主持人要求補充 BIAS Amp Match 深入資料（13_bias_amp_match.md；Docs 第七章加一小節）
- [x] 2026-09-27 應主持人要求補充 NAM 源頭／訓練修正方式／A2 問題（14_nam_origin_training_a2.md；Docs 第四章加一小節）
- [x] 2026-09-28 應主持人要求補充 Fractal Tone Match 與可微分白盒（15_…md；Docs 第七章加一小節）
- [ ] 2026-09-29 推送到 GitHub repo（Pedal-Guy/Podcast_Modeling_n_Profiling）：README.md、.gitignore 已準備；雲端 Session 無此 repo 的推送權限，需在本機推送或把 repo 加入 Session 來源
- [ ] 等待主持人回饋：單人或雙人主持、節目名稱、是否插入盲測音訊、時長要壓到 90 分鐘或拉到 120 分鐘

## 已知的待確認事項（UNVERIFIED）
- POD 上市年份：多數來源 1998，Line 6 官網時間軸寫 1999。
- Helix Stadium 發表日（6/11 vs 6/13 2025）、Proxy 韌體 1.3 日期（2026/3/24 vs 4/2）。
- Kemper 核心 profiling 專利確切號碼與到期日（Google Patents: US 8,796,530 B2，優先權 2006-07-29）。
- A2 盲測為 TONE3000 自辦，非同行審查；原始數據公開（github.com/tone-3000/a2-mushra-data）。
- ToneHunt 最初創辦人未確認（repo 與 Oli Larkin 有關）。
- 使用者提到的「ShowMe」skill 在本 Session 的技能清單中找不到；圖表改以 Claude Docs 互動圖繪製，並匯出 PNG 與 Mermaid 原始碼。

## 下一步建議
1. 主持人讀 10_script.md 與 09_episode_rundown.md，標記要刪減/加強的段落。
2. 決定是否邀請來賓（講稿目前以「主持人＋可選來賓提問」撰寫）。
3. 若要做節目 show notes，可直接從 12_references.md 與 03_timeline.md 擷取。

## 給下一個 Session 的工作方式
- 以本資料夾 Markdown 為準；若要改講稿，先改 10_script.md，再同步到 Docs 文件（或反之，並註明）。
- 講稿中的數字都附在 10_script.md 最後的「關鍵數字速查」，出處細節在 01_research_raw/。
- 主持人任職 Positive Grid，講稿中已標記提到 BIAS／Spark 時的揭露提示。
- 未來其他主題請在 Podcast/ 下另開 subfolder，沿用本資料夾結構（00_README_HANDOFF、01_research_raw、編號檔、diagrams/）。
