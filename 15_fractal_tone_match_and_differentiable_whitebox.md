## 補充：「以模型為底」的兩種做法——Fractal Tone Match 與可微分白盒

一句話：**Fractal Tone Match 和 BIAS Amp Match 是同一家族：模型照跑，量兩邊的頻譜差，在後面補一個線性濾波器，修的是「聲音的顏色」。可微分白盒（Native Instruments 研究團隊 2020–2021）則完全不同：電路方程式本身變成可訓練的，用錄音去反推電阻、電容、電位器曲線、二極體特性，修的是「電路裡的元件」。**前者今天就能在產品上用；後者是研究方法，也是你「modeling 為底、profiling 修正」想法最接近的學術版本。

---

### 一、Fractal Tone Match

**是什麼**：Axe-Fx II 從韌體 6.00 起加入的「Tone Match」模組（約 2012 年；確切日期未查證），之後延續到 Axe-Fx III 與 FM9。官方手冊的定義：「Tone Match 模組會改變 Axe-Fx preset 的聲音，讓它符合一個參考訊號，例如一段錄音，或真音箱麥克風的即時訊號。」

**原理**：官方 Amp Matching 教學說它是「一個強大的 FFT 差異分析器，找出參考訊號與本地訊號之間的頻譜差異」。

- **Reference（參考）**：目標聲音，可以是錄音檔（USB、錄音介面），或麥克風收的真音箱。
- **Local（本地）**：你目前的 preset（Axe-Fx 的音箱模型）。
- 兩邊各分析約 10 秒的平均頻譜（可調長，最長會切到「峰值保持」），只做單聲道。
- 算出兩條頻譜的差，做成一個修正濾波器，放在 preset 裡（通常在喇叭箱模組之後）。

**操作步驟**（Axe-Fx II 手冊）：

1. 先建立起點：選「同型或相似的音箱」，把 tone 與 drive／gain 調到接近。手冊說：「越接近越好，但 Tone Match 是用來縮小差距的，不必鑽牛角尖。」
2. 前面不要有效果器。
3. 按 X 擷取參考、按 Y 擷取本地，彈一樣的和弦或樂句。
4. 按 ENTER 產生 Tone Match。
5. 用 **Amount（0–100%）** 控制修正強度、**Smoothing** 抹平細小起伏（「先抹掉小漣漪，最後只保留極端的峰谷」）。

**兩個關鍵細節**：

- **即時比對比錄音比對更準**：教學說即時模式下「同一個激勵訊號可以同時送進真音箱和 Axe-Fx」。你彈的每一下同時進兩邊，差異就只剩「音箱 vs 模型」，不會混進演奏差異。
- **比對真音箱時，先抓喇叭 IR**：手冊建議「比對真音箱時，先為它的喇叭做一個自訂 IR」。教學給兩種方法：
    - 方法一：先抓喇叭箱 IR，再用 Tone Match 修剩下的差，IR 可以給其他音箱模型重複使用。
    - 方法二：直接 Tone Match，喇叭和音箱的差異混在一起修，「音箱模組和 Tone Match 模組要留在同一個 preset」。
- **結果可以存成 User Cab IR**：手冊提到可以把比對結果匯出成喇叭 IR。這說明修正層本質上就是一個**線性濾波器**——一個 IR。

**限制**（手冊原文）：

- 錄音品質：「爛錄音裡的好音箱是不行的！」
- 必須是乾淨、單獨的吉他：其他樂器、人聲、雜訊或部分效果會「污染」比對。
- 要有多樣的內容：「十秒多樣的和弦與句子，比六十秒的長音更能說明音箱怎麼反應。」
- 音量差異影響很大：「即使很小的音量差，也會大幅影響感受。」
- 本質限制（推論）：它修的是頻率響應，不改變模型的破音結構和動態；只在比對時那組設定最準。

**和 BIAS Amp Match 的比較**：

| | Fractal Tone Match | BIAS Amp Match |
| --- | --- | --- |
| 推出 | Axe-Fx II 韌體 6.00（約 2012） | BIAS Desktop Professional（2014–15） |
| 比對對象 | 錄音、或麥克風即時訊號（同一激勵同時送兩邊） | 錄音、或麥克風收的真音箱 |
| 修正層 | FFT 頻譜差 → 濾波器，可匯出為 IR | 「音色補償」→ EQ 曲線 |
| 可調 | Amount、Smoothing、平均時間 | 五顆微調旋鈕 |
| 起點要求 | 同型或相似音箱 | 接近的 BIAS 預設 |
| 本質 | 模型＋線性修正 | 模型＋線性修正 |

---

### 二、可微分白盒（Differentiable White-Box Virtual Analog Modeling）

**是誰**：Fabián Esqueda、Boris Kuznetsov、Julian D. Parker，**Native Instruments**（柏林），發表於 DAFx 2020–21。前一篇〈Differentiable IIR Filters for Machine Learning Applications〉（DAFx 2020，同一團隊）是它的基礎。

**要解決的問題**：白盒模擬需要準確的電路圖和元件數值，但實際上常常拿不到，或者跟實機有誤差（元件公差、老化）。論文的說法：「把白盒模型寫成可微分的形式，就能從原始輸入輸出錄音中學出近似的元件數值。」

**核心概念，用白話講**：

1. **電路照樣寫成方程式**：用狀態空間表示「電壓怎麼隨時間變化」，未知的元件數值（電阻、電容等）當作可調參數 λ。
2. **離散化**：用梯形法則（線性時等同雙線性轉換）把連續時間方程式變成每個取樣點可以算的式子，這種方法本身就穩定。
3. **非線性元件**：二極體用 Shockley 方程式，每個取樣點要用 Newton-Raphson 反覆求解。
4. **讓整條計算「可微分」**：在 PyTorch 裡實作，框架會自動算出「如果某顆電阻大一點，輸出誤差會怎麼變」，連 Newton-Raphson 的迭代也能反向傳遞。
5. **用錄音訓練**：輸入真實電路的乾訊號、比較模型與實機輸出（損失用時域 MSE），以梯度下降調整元件數值。
6. **兩個實務技巧**：
    - 元件數值跨好幾個數量級（kΩ、nF），所以不直接學原值，而是學「縮放倍率」，讓訓練穩定。
    - 使用者可調的旋鈕（電位器），用一個很小的神經網路學它的曲線，例如 f(x) = w₁·tanh(w₂x + b₂) + b₁，把旋鈕位置對應到阻值。

**三個案例與結果**：

| 案例 | 做法 | 結果 |
| --- | --- | --- |
| RC 低通濾波器（12 kΩ、68 nF，約 195 Hz） | 故意從錯誤數值（4.7 kΩ、47 nF）開始訓練 | 學到的截止頻率約 224 Hz，與標稱差 29 Hz，反映元件公差；頻率響應與實測吻合 |
| FMV Tone Stack（Fender／Marshall／Vox 共用的音色控制電路） | 用鱷魚夾從一台真實 Marshall 風格音箱量 6 組旋鈕設定；小網路學對數電位器曲線 | 多組設定的頻率響應都更吻合；學到的數值與公開規格略有差異（例如 VR1 250k→312k、C1 250pF→327.5pF） |
| Ibanez TS-808 破音級 | 兩顆二極體分開建模（抓不對稱）；3 個 overdrive 位置、2 分鐘錄音 | 誤差從 6.4×10⁻³ 降到 1.5×10⁻³；學出二極體的不對稱特性 |

**限制**（論文自述）：

- **答案不唯一**：例如 RC 濾波器，無限多組 R、C 會得到同一個截止頻率。所以學到的數值不一定是實體元件的真值。
- **學到的數值會「吸收」離散化誤差**：系統會調整參數來補償數值方法的不準，進一步降低可解釋性。
- **長序列的梯度問題**：梯度消失、記憶體與計算量大增。
- **非線性求解很慢**：Newton-Raphson 讓訓練時間大幅增加。
- 論文驗證的是單一電路級；整台音箱（多級、回授、後級與喇叭互動）還沒做到。

**同一路線的其他研究**：

- **Differentiable IIR Filters（DAFx 2020）**：證明傳統 IIR 濾波器就是一種線性 RNN，可以直接訓練；用「濾波器→小神經網路→濾波器」（Wiener-Hammerstein）模擬 Boss DS-1，不需要特殊量測訊號就達到與既有方法相當的準確度。
- **Differentiable Wave Digital Filters（Chowdhury & Clarke，SMC 2022）**：把波數位濾波器也變成可微分，用來模擬二極體電路，並提供 JUCE 即時外掛與開源程式碼。
- **Neural Grey-Box（Miklánek 等，DAFx 2023）**：前級與後級用神經網路、tone stack 用可微分白盒；只錄一組設定 4 分鐘，就能推廣到沒錄過的 tone stack 設定。

---

### 三、三種「以模型為底」的做法放在一起看

| | BIAS Amp Match | Fractal Tone Match | 可微分白盒 |
| --- | --- | --- | --- |
| 修正什麼 | 輸出的頻譜（EQ） | 輸出的頻譜（濾波器／IR） | 電路裡的元件數值與旋鈕曲線 |
| 修正後轉旋鈕 | 修正曲線不跟著變 | 修正曲線不跟著變 | 仍然正確，因為改的是電路本身 |
| 能修破音與動態嗎 | 不能 | 不能 | 可以（例如學出二極體不對稱） |
| 需要什麼資料 | 兩段彈奏錄音 | 兩段彈奏或即時訊號 | 實機的乾／濕錄音，最好多組旋鈕設定 |
| 需要知道電路嗎 | 只要選相近模型 | 只要選相近模型 | 必須有電路拓撲 |
| 成熟度 | 已商品化（2014–） | 已商品化（約 2012–） | 研究階段（2020–） |

**節目中的一句話**：「Tone Match 和 Amp Match 是在音箱後面加一副有色眼鏡；可微分白盒則是打開音箱，把每顆零件換成跟你那台一模一樣。」

**和你的想法的關係**：你提出的「modeling 為底、profiling 修正」，如果修正層只到 EQ，就是 Tone Match／Amp Match，2012 年就有了；如果修正層深入到元件數值，就是可微分白盒，這在研究上可行，但要做成產品，還需要解決整台音箱的規模、回授迴路與訓練成本。

### 資料來源

- [Fractal Audio：Axe-Fx II Tone Match 手冊](https://www.fractalaudio.com/downloads/manuals/axe-fx-2/Axe-Fx-II-Tone-Match-Manual.pdf)
- [Fractal Audio：Amp Matching Tutorial（韌體 6.00 起）](https://www.fractalaudio.com/downloads/manuals/axe-fx-2/Amp%20Matching%20Tutorial.pdf)
- [Fractal Audio：Blocks Guide（Tone Match 支援機型）](https://www.fractalaudio.com/downloads/manuals/fas-guides/Fractal-Audio-Blocks-Guide.pdf)
- [Gearspace：Axe Fx II version 6 Tone Matching 討論串](https://gearspace.com/board/all-things-technical/723689-axe-fx-ii-version-6-tone-matching.html)
- [Esqueda, Kuznetsov, Parker：Differentiable White-Box Virtual Analog Modeling（DAFx 2020–21）](https://dafx.de/paper-archive/2021/proceedings/papers/DAFx20in21_paper_39.pdf)
- [Kuznetsov, Parker, Esqueda：Differentiable IIR Filters for Machine Learning Applications（DAFx 2020）](https://www.dafx.de/paper-archive/2020/proceedings/papers/DAFx2020_paper_52.pdf)
- [Chowdhury & Clarke：Differentiable Wave Digital Filters（SMC 2022）程式碼](https://github.com/jatinchowdhury18/differentiable-wdfs)
- [Chowdhury & Clarke：Emulating Diode Circuits with Differentiable WDFs（Zenodo）](https://zenodo.org/records/6566846)
- [Miklánek 等：Neural Grey-Box Guitar Amplifier Modelling with Limited Data（DAFx 2023）](https://www.dafx.de/paper-archive/2023/DAFx23_paper_52.pdf)
