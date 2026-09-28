## 補充：NAM 的源頭、模型如何「修正」自己、A2 想解決的問題

一句話：**NAM 是 Steven Atkinson 在 2019 年用「閉卷」方式做的週末專案，2022 年才開始參考學術架構；它的「修正方式」是用梯度下降反覆比對「模型的猜測」與「真音箱的錄音」，逐步調整上萬個權重；A2 則是為了讓同一個模型能在 3 美元晶片上原生執行、同時修掉 A1 在高增益音色上的金屬感殘響，並把時間記憶拉長到能抓住 sag 與壓縮。**

### 一、源頭：從「閉卷」週末專案到開源標準

| 時間 | 事件 | 意義 |
| --- | --- | --- |
| 2019 | Atkinson（機器學習博士、當時是科學家）開始專案並開源 | 動機是把工作上的 ML 技能用在吉他嗜好上；「第一次嘗試就效果不錯」 |
| 2020/2/21 | v0.1.0（TensorFlow 1） | 之後沉寂約兩年 |
| 2022/2 | 「第二春」：改用 iPlug2 做成真正的外掛 | 重點從研究轉向可用性 |
| 2022 初 | 與 Kemper 比較 | 量化結果「NAM 其實比 Kemper 更準」 |
| 2022/5 | 與 Quad Cortex 比較 | 他原本預期會輸，結果 NAM 更好，於是認為「NAM 是神經建模的最先進水準」 |
| 2022/8 | 與 TONEX 比較 | 「NAM 還是比較好」 |
| **2022/9** | **第一次「打開書本」**：開始讀既有研究，實作 LSTM 與 WaveNet，並依自己的直覺修改 WaveNet | 也就是說，前面三場比較用的是他自己摸索出的架構 |
| 2022/11 | 通用模型載入器 | 使用者不必編譯程式就能載入任何模型 |
| 2023 | iPlug2 作者 Oli Larkin 主動重構外掛，加入 EQ、IR loader；瀏覽器與本機 GUI 訓練器；Facebook 社團年底破 1.5 萬人 | ToneHunt、Poly Effects Beebo、MOD Dwarf、MeldaProduction 等生態系出現 |
| 2025/5 | Atkinson 全職投入 NAM | |
| 2025/11 | 論文〈Slimmable NAM〉 | A2 的理論基礎 |

**節目可以強調的點**：NAM 並不是照著 Aalto 論文實作出來的。它是一條平行路線，到 2022 年 9 月才和學術界接軌。Atkinson 自己說，一個「好玩的專案」贏過商業產品，「真的讓我很意外」。他也提到，大家對 NAM 的認識多半來自社群媒體，所以像參數化模型這類重要進展反而很少人知道。

### 二、「修正方式」：NAM 怎麼讓模型越來越像真音箱

先講直覺：**神經網路一開始是一堆隨機數字。它每次猜一段音箱輸出，跟真實錄音比較，算出差多少，再把每個權重往「讓差距變小」的方向微調。重複幾萬次後，它就學會了這台音箱。**這跟 Kemper 不一樣：Kemper 調的是一個有意義的放大器參數（增益、EQ、sag…），NAM 調的是上萬個沒有電路意義的權重。

NAM 訓練器（開源程式碼 `nam/train/core.py`）實際做了這些步驟：

1. **辨識測試訊號版本**：用檔案雜湊值比對是哪一版標準測試訊號（v1–v3，另有相容 Proteus 的格式）。目前 v3 約 3 分鐘，結構是：驗證段（0:00–0:09）→ 靜音 → 定位脈衝「blips」（0:10–0:12）→ 掃頻 → 雜訊 → 一般訓練用的吉他演奏。
2. **修正延遲（latency calibration）**：錄音介面與音箱會讓輸出比輸入晚一點點。訓練器用 blips 自動找出延遲，把兩個檔案對齊（預留 1,000 個樣本的前看量），數值可疑時會警告。沒對齊的話，模型會學到錯的東西。
3. **資料檢查**：驗證段在錄音中會出現兩次，訓練器比較這兩段；如果差異（ESR）超過 0.01，就提示「你的器材彈兩次聽起來不一樣！」。這能抓到雜訊、器材不穩定或 reamp 設定錯誤。
4. **訓練（真正的修正迴圈）**：
    - 最佳化器：Adam，學習率 0.004，每個 epoch 以指數衰減（A1 預設 γ=0.993，A2 設定 γ=0.994），預設 100 個 epoch，每批 16 段、每段 8,192 個樣本。
    - 損失函數（衡量「差多少」）：以波形誤差為主；A2 的預設設定另加上多解析度頻譜損失（MRSTFT，權重 0.0005），讓模型也注意頻譜上的差異，不只波形。程式也支援預強調（pre-emphasis，建議係數 0.95），讓高頻錯誤更被重視——這是沿用 Aalto 論文的做法。
    - 驗證指標：ESR（誤差能量 ÷ 真實訊號能量）。
5. **評分**：訓練器用 ESR 給出白話評語：小於 0.01「很棒」、小於 0.035「不錯」、小於 0.1「可能還可以」、小於 0.3「大概不太好聽」，更高則「可能哪裡出錯了」。
6. **輸出與播放端修正**：產生 .nam 檔，metadata 記錄擷取時的輸入與輸出電平（dBu）。播放器用這些數值做「輸入校準」，把你的吉他電平調到跟擷取時一樣。這是在回應社群早期的抱怨：不同人做的模型增益不一致，很難信任。

**一句話對比**：Kemper 是「把量測結果套進一個有意義的放大器模型」；NAM 是「讓一個沒有預設結構的網路自己學會輸入輸出關係」；BIAS Amp Match 是「模型不變，只在後面加一條 EQ」。

### 三、A1 長什麼樣、問題在哪

| 項目 | A1-Standard（2022–2026 主力） | A2（2026/6/2） |
| --- | --- | --- |
| 結構 | 兩組 WaveNet 層陣列：16 通道＋8 通道，各 10 層 | 單組 23 層；Full 用 8 通道、Lite 用 3 通道，同一檔案 |
| 卷積核 | 3 | 大多 6，中段兩層 15（混合大小） |
| 擴張（dilation） | 1、2、4…512（每層加倍） | 1、3、7、17、41、101、239（手調、不加倍），中間插入去格化層 |
| 激活函數 | Tanh | LeakyReLU |
| 輸出頭 | 逐樣本 | 看 16 個樣本的卷積頭 |
| 感受野 | 約 4,100 樣本（約 85 ms） | 約 6,350 樣本（約 132 ms） |
| 參數量 | 約 1.38 萬 | 官方未公布 |
| TONE3000 資料庫 ESR 中位數 | 0.0062 | 0.0033（A2-Full） |

A1 的四個問題：

1. **太吃運算**：Atkinson：「1.38 萬個參數，以嵌入式標準來說是一大塊運算。」踏板、樹莓派只能跑縮小版（MOD Dwarf 只跑 nano、Beebo 只跑 feather）。
2. **NAM-to-X 的品質損失**：Hotone、Valeton、NUX 等廠牌把 .nam 轉成自家格式來跑，只是近似。
3. **高增益的金屬感**：TONE3000 指南說 A1 有「類似混疊的瑕疵」與「ring 瑕疵」，在高增益音箱上聽起來像「金屬般的鳴響」。原因之一是每層加倍的擴張造成的格狀（gridding）效應與諧波堆積。
4. **記憶不夠長**：85 ms 不足以完整描述 sag、壓縮這類隨時間變化的行為。

另外還有一個流程問題：若要給不同硬體不同大小的模型，得訓練好幾次，或做「蒸餾」；論文指出，沒有 GPU 的使用者做蒸餾很麻煩。

### 四、A2 的解法，對應每個問題

| 問題 | A2 的做法 |
| --- | --- |
| 太吃運算 | Tanh 換成更便宜的 LeakyReLU，省下的算力拿去加大網路；混合卷積核大小降低推論 CPU；Full 比 A1-Standard 效能好約 30–40% |
| 硬體跑不動、NAM-to-X | A2-Lite 在 600 MHz Cortex-M7 上約 50% CPU，可原生執行，不必轉檔；新增 .namb 二進位格式、純 C 引擎、WASM 引擎 |
| 一個模型要多種尺寸 | 可瘦身（slimmable）＋打包訓練（packed training）：Full 與 Lite 是同一模型的兩個「瘦身點」，各有自己的權重區塊、一起訓練 |
| 高增益金屬感 | 手調、不加倍的擴張序列＋去格化層，減少 ring 堆積；卷積輸出頭讓混音更有表現力 |
| 記憶不夠長 | 感受野拉長到約 132 ms，抓 sag 與壓縮 |
| 舊模型怎麼辦 | A1 繼續支援；TONE3000 重新訓練資料庫，沒有原始錄音的就用 A1 生成合成資料訓練（標示「Convert」） |

**開發過程**（Atkinson 的公開計畫）：

1. 2026/1/17 公布計畫，TONE3000 資助；限制條件是訓練維持約 10 分鐘、錄音約 3 分鐘，而且「沒有東西會被拿掉」。
2. 先在 NeuralAmpModelerCore v0.4.0 加入所需 DSP 功能，讓硬體廠先整合。
3. 約 2 月在目標硬體上實測 CPU（Dimehead、樹莓派、電腦、網頁），畫出「架構參數 vs CPU」的關係。
4. 3/5 公布早期發現：小網路上 LeakyReLU 在同樣 CPU 下更準；瓶頸層沒幫助；FiLM 條件調變效果很小。
5. 3–4 月聽感測試（MUSHRA，1,000 人以上）。
6. 6/2 正式發布。

### 五、節目裡可以這樣講

- **源頭**：「一個科學家在不讀論文的情況下，做出了比商業產品更準的東西；直到 2022 年 9 月才『打開課本』。」
- **修正方式**：「它不是在調旋鈕，而是在調一萬多個沒有名字的數字；每一次都問自己：我猜的聲音和真音箱差多少？」
- **A2**：「A1 準，但太重、高增益有金屬味、記憶只有 85 毫秒；A2 把它變輕、變長、變乾淨，而且一個檔案同時給電腦和 3 美元的晶片用。」
- **提醒**：A2 的盲測是 TONE3000 自辦；Atkinson 部落格的正式發布文章本身沒有列出數字，技術數字主要來自 TONE3000 的指南與開源程式碼。

### 資料來源

- [Atkinson：The History of NAM](https://www.neuralampmodeler.com/post/the-history-of-nam)
- [Atkinson：Architecture "A2"（2026/1/17）](https://www.neuralampmodeler.com/post/architecture-a2)
- [Atkinson：An Early Glimpse at A2（2026/3/5）](https://www.neuralampmodeler.com/post/an-early-glimpse-at-a2)
- [Atkinson：A2 is released（2026/6/2）](https://www.neuralampmodeler.com/post/a2-is-released)
- [TONE3000：NAM A2 完整指南](https://www.tone3000.com/guides/nam-a2-the-complete-guide)
- [Atkinson：Slimmable NAM（arXiv 2511.07470）](https://arxiv.org/html/2511.07470v1)
- [NAM 訓練器程式碼：nam/train/core.py](https://raw.githubusercontent.com/sdatkinson/neural-amp-modeler/main/nam/train/core.py)
- [NAM 訓練器程式碼：lightning_module.py（損失函數）](https://raw.githubusercontent.com/sdatkinson/neural-amp-modeler/main/nam/train/lightning_module.py)
- [A2 預設設定：config_model_packed.json](https://raw.githubusercontent.com/sdatkinson/neural-amp-modeler/main/nam/train/_resources/config_model_packed.json)
- [A1 設定（v0.12.0 core.py）](https://raw.githubusercontent.com/sdatkinson/neural-amp-modeler/v0.12.0/nam/train/core.py)
- [.nam 檔案格式](https://neural-amp-modeler.readthedocs.io/en/latest/model-file.html)
- [Wright 等 2020（ESR 與預強調的來源）](https://www.mdpi.com/2076-3417/10/3/766)
