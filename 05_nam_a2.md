## 四、NAM 與 A2（Full／Lite）

重點：**NAM 是 Steven Atkinson 2019 年起的個人開源專案，用 WaveNet 類神經網路擷取音箱，2023 年爆紅；A2 是 2026 年 6 月 2 日發布的第二代架構，由 TONE3000 全額資助，目標是「同樣或更準、更省 CPU」，讓一個檔案同時能在電腦（A2 Full）和便宜的踏板晶片（A2 Lite）上跑。**

### NAM 是什麼、從哪來

- **作者**：Steven Atkinson，機器學習博士、曾任 Amazon 應用科學家；2025 年 5 月起全職投入 NAM（Atkinson Advanced Modeling, LLC）。
- **起點（2019）**：他在〈The History of NAM〉寫道，身為吉他手又是做機器學習的科學家，「想看看能不能把這些技能用在音樂嗜好上」，而且刻意「閉卷」、像解填字遊戲一樣自己摸索。
- **動機**：擁有自己的聲音、而且開放。他說：「如果你愛你音箱 chug 的聲音，就錄下自己 chug，從此那個聲音永遠是你的。」「就算我明天停手，程式碼也還在那裡。」——對比的是 Kemper、Quad Cortex 這類封閉、昂貴的系統。
- **發展**：2020/2/21 首個版本（TensorFlow 1）→ 2022 年 2 月「第二春」，做出 iPlug2 外掛並發表與 Kemper、QC、TONEX 的比較 → 2022 年 11 月通用模型載入器 → 2023 年初爆紅（Facebook 社團年底破 1.5 萬人），iPlug2 作者 Oli Larkin 加入 EQ 與 IR loader。
- **授權**：MIT 開源。三個部分：訓練器（Python/PyTorch，可用 Colab 或本機 GUI）、外掛（現以「Gateway」名稱發行）、C++ 核心函式庫 NeuralAmpModelerCore（硬體廠商內嵌用）。

### 技術核心

- **原理**：黑盒神經網路。播放官方標準測試訊號（input.wav）經過真實音箱（reamping），錄下等長的輸出，用這對檔案訓練模型學「輸入→輸出」。
- **設計目標**：錄音約 3 分鐘、訓練約 10 分鐘。
- **架構**：預設 WaveNet（擴張因果卷積）；A1 時代有 standard（約 1.38 萬參數）、lite、feather、nano 四種大小；也支援 LSTM。
- **準確度**：以 ESR（誤差訊號比）衡量，越低越好。
- **.nam 檔**：JSON 格式，含 version、architecture、config、weights（學到的權重），以及選填 sample_rate（預設 48 kHz）與 metadata（名稱、作者、器材、類型、input/output dBu 校準值）。

### 有沒有論文基礎？

- **沒有一篇「創始論文」**。NAM 屬於 Aalto 大學那條研究線（Damskägg 2019 SMC 最佳論文、Wright 2020 Applied Sciences）的同路人，但 Atkinson 表示他並非照著那些論文實作，屬於平行發展。
- Atkinson 自己的論文：**〈Slimmable NAM: Neural Amp Models with adjustable runtime computational cost〉**（arXiv 2511.07470，2025 年 11 月）——這正是 A2「一個模型、多種尺寸」的理論基礎。

### A2：為了解決什麼？

- **時程**：2026/1/17 Atkinson 部落格宣布、1/20 TONE3000 宣布「全額資助 A2 開發」**，2026/6/2 正式發布**（trainer v0.13.0、Core v0.5.2、plugin v0.7.14）。
- **背景需求**：NAM 已經跑在電腦、樹莓派、踏板、網頁上（Atkinson：「音訊界很少有技術被用在比 NAM 更多的地方」），但 A1 的 standard 模型太吃 CPU，踏板廠只能跑縮水版或自行轉檔（Atkinson 稱之為「NAM-to-X」，只是近似）。
- **四個目標**：維持或提升準確度；大幅降低 CPU（他說這是「最有趣的方向」）；訓練維持約 10 分鐘；錄音維持約 3 分鐘。A1 繼續支援（「Nothing is going away」）。

### A2 Full 與 A2 Lite

- **不是兩種格式，而是同一個檔案裡的兩種尺寸**：「一個 A2 下載就是一個 NAM 模型，可以用 A2-Full 或 A2-Lite 執行」。技術上是「可瘦身（slimmable）WaveNet」：Full 用 8 個通道、Lite 用 3 個，兩組權重一起訓練（packed training）。
- **A2 Full**：給錄音室／電腦；官方稱效能比 A1-Standard 好約 30–40%。
- **A2 Lite**：給踏板與音箱；在 600 MHz 的 ARM Cortex-M7 上約佔 50% CPU，官方口號是「能在 3 美元晶片上跑」；聽感約等同 A1-Standard。一台 M 系列 MacBook 可同時跑約 64 個 Full 或 200 個 Lite。
- **技術改動**：激活函數 Tanh→LeakyReLU（便宜，省下的算力拿去加大網路）；輸出端改看一段樣本窗；感受野從約 85 ms 增到約 132 ms（6,350 樣本 @48 kHz）；訓練資料正規化到 −18 dB RMS；新增給嵌入式用的二進位 **.namb** 格式。TONE3000 資料庫的 ESR 中位數從 0.0062 降到 0.0033。
- **相容性**：「A2 是新架構，不是直接升級」，硬體要更新韌體。TONE3000 把整個資料庫重新訓練；沒有原始錄音的舊模型，則用 A1 模型生成合成資料再訓練（標示為「Convert」）。
- **全部 MIT 開源**：核心、訓練器、嵌入式載入器、純 C 引擎、WASM（網頁）引擎；TONE3000 另開源參考踏板設計（STM32H750）。

### 盲測與爭議（節目中要講清楚）

- TONE3000 自辦 MUSHRA 盲測：1,184 人、105,842 筆評分、37 個音色、7 個系統；原始資料以 CC-BY-4.0 公開。
- Guitar World 報導的相對分數：A2 Full 100、Neural DSP V2 94、TONEX 91、Line 6 Proxy 77（未含 Kemper）。
- **但這是廠商自辦、非同行審查**。The Gear Forum 網友重新分析後認為，排除不可靠評分者後 A2-Full 與 A1 的差距（92 vs 88）「沒那麼大」，A2-Lite 與 QC V2「差不多」。

### 限制與下一步

- 標準 NAM 仍是**單一設定快照**。Atkinson 做過參數化模型（2024 年免費的 ParametricOD；授權給 SubMission Audio PreFire 等商品），但參數化訓練不在開源訓練器的預設流程中。
- 學界的 PANAMA（ETH，2025）用約 75 組旋鈕設定就能訓練參數化模型。
