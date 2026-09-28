## 三、技術深入：白盒、黑盒、灰盒與 IR

核心觀念：**複製一台音箱有三條路——白盒（照電路重建）、灰盒（用「濾波器→失真→濾波器」的簡化結構去擬合）、黑盒（只看輸入與輸出，讓神經網路學）**。Modeling 主要走白盒與灰盒，Kemper profiling 是灰盒，NAM／Quad Cortex／TONEX 是黑盒。IR 則是三條路都會用到的線性積木。

### 1. 音箱到底在做什麼（訊號鏈）

吉他 → **前級真空管**（主要失真來源）→ **Tone stack**（Bass／Mid／Treble，被動濾波器）→ **倒相器** → **後級功率管** → **輸出變壓器** → **喇叭箱** → **麥克風**。

- 前級到輸出變壓器：**非線性**（會削波、產生諧波、彈越大力音色越變），而且**有記憶**（電源 sag、耦合電容充電造成偏壓漂移）。
- 喇叭箱＋麥克風：在正常音量下**近似線性**，一個 IR 就能描述。
- 各級之間**互相影響**：例如喇叭的阻抗隨頻率變化，會回頭改變後級的反應（Line 6 Agoura 特別強調這點）。

### 2. 白盒：電路模擬（Modeling 的核心）

- 做法：拿電路圖，把每個電阻、電容、真空管寫成方程式，即時求解。
- 方法：SPICE 式節點分析、狀態空間、**DK 方法**（David Yeh，Stanford 2009／IEEE TASLP 2010）、**波數位濾波器 WDF**（Fettweis 1986；Karjalainen & Pakarinen 2006 用於真空管）。真空管本身常用 **Koren 模型**（1996）。
- 優點：每個旋鈕都「是真的」；可以模擬沒實物的音箱，甚至設計不存在的音箱（Positive Grid BIAS 的元件級設計概念）。
- 缺點：運算量大；每台音箱都需要專家投入；元件公差與老化難以涵蓋。
- 業界實務其實是混合：Fractal 的 Cliff Chase 說他們的三極管模型「有記憶」，不是靜態轉換曲線；Line 6 說元件級模型讓 Presence 旋鈕「真的在控制後級負回授」。

### 3. 灰盒：區塊模型（Kemper 與早期研究）

- 把音箱看成「**線性濾波器 → 非線性失真 → 線性濾波器**」，稱為 **Wiener-Hammerstein** 模型（Wiener＝L–N，Hammerstein＝N–L）。
- 線性部分用掃頻量測，非線性部分擬合一條失真曲線。
- 學界文獻（Vanhatalo 2022 綜述、Eichas DAFx-17）指出 Kemper 早期專利描述的正是這類灰盒結構；Kemper 自己從未公開內部細節。
- Kemper 的做法：送出由小到大的測試訊號，推算「失真如何隨輸入大小改變」，把結果擬合到自家可調的內建放大器結構上。所以 profile 之後仍能調 sag、壓縮、pick attack。論壇有人形容「Kemper 其實是 modeler，只是 profiling 時自動幫你寫好參數」。
- 限制：高增益與極端設定較難（Eichas 2017 研究中重失真評分中位數約 50）；轉 tone stack 無法重現真機行為（因此才有 Liquid Profiling）。

### 4. 黑盒：神經網路（Capture 與 NAM）

- 做法：錄一段標準測試訊號（乾訊號）與它經過音箱後的輸出，訓練神經網路學會「輸入→輸出」。
- 兩種主流架構：
    - **WaveNet（擴張因果卷積）**：看過去一段時間窗的樣本來決定現在的輸出；NAM 預設架構。
    - **LSTM／GRU（遞迴網路）**：有內部狀態，天生適合「有記憶」的系統；Aalto 2019 發現 LSTM 用少很多的 CPU 就能接近 WaveNet。
- 準確度指標 **ESR（誤差訊號比）**：誤差能量 ÷ 真實訊號能量，0 為完美；0.01 代表 1%。研究發現要加上聽覺加權（A-weighting）才貼近耳朵感受。
- 歷程：Covert & Livingston 2013「效果不如預期」→ Schmitz & Embrechts 2018 LSTM → Damskägg 等 2019 WaveNet（ICASSP）→ Wright 等 2020「盲測聽眾分不出來」→ 2023 以後 GAN、時間調變、灰盒混合。
- 限制：標準擷取是**單一設定快照**；要可調就要錄很多組設定（TINA 機械手臂）或用參數化模型。

### 5. IR 與卷積（兩邊共用的積木）

- **IR**：對系統送一個極短的脈衝，錄下的回應就是它的「指紋」。對線性非時變（LTI）系統，這個指紋就能完整描述它。
- **卷積**：把輸入的每個樣本都乘上一份延遲過的 IR 再疊加；IR loader 做的就是這件事。
- **量測**：實務上不用脈衝，而是 Farina（2000）的指數正弦掃頻，還能把諧波失真分離出來。
- **為什麼只適合喇叭箱**：喇叭箱＋麥克風接近線性；音箱會失真、會 sag。Guitar World：「IR 抓喇叭箱非常準，但它是線性的」。喇叭紙盆破音、音圈發熱壓縮屬於音量相關的非線性，IR 只在一個音量下量得。
- **Two Notes DynIR**：用大量靜態 IR（每箱約 16 萬個）讓你即時移動麥克風，但仍是線性。

### 6. 抗混疊（每一種數位失真都要面對）

失真會產生超過取樣頻率一半的諧波，數位系統會把它們「折返」成不和諧的雜音。Line 6 1998 年專利的關鍵就是在非線性處理前後做 **8 倍超取樣**。這也是為什麼好的 modeler 很吃 DSP。

### 關鍵論文速查

| 年份 | 作者 | 論文 | 意義 |
| --- | --- | --- | --- |
| 1986 | Fettweis | Wave Digital Filters: Theory and Practice | WDF 基礎 |
| 1996 | Koren | Improved vacuum tube models for SPICE | 標準真空管模型 |
| 2000 | Farina | Swept-sine IR & distortion measurement | IR 量測標準 |
| 2009 | Pakarinen & Yeh | A review of digital techniques for modeling vacuum-tube guitar amplifiers | 前神經網路時代最重要綜述 |
| 2010 | Yeh, Abel, Smith | Automated physical modeling of nonlinear audio circuits (DK method) | 自動化電路模擬 |
| 2013 | Covert & Livingston | Vacuum-tube amp model using RNN | 第一篇神經網路音箱 |
| 2019 | Damskägg et al. | Deep learning for tube amplifier emulation | WaveNet 音箱 |
| 2019 | Wright et al. | Real-time black-box modelling with RNNs | LSTM 即時 |
| 2020 | Wright et al. | Real-time guitar amplifier emulation with deep learning | 盲測難分辨 |
| 2020 | Düvel et al. | Confusingly similar（Kemper 盲測） | 177 人，56.2% |
| 2022 | Vanhatalo et al. | A review of neural network-based emulation of guitar amplifiers | 神經網路綜述 |
| 2023 | Miklánek et al. | Neural grey-box guitar amplifier modelling with limited data | 灰盒混合 |
| 2025 | Atkinson | Slimmable NAM | A2 的理論基礎 |
