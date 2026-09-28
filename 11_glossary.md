## 十、技術名詞簡易說明

依節目出現順序排列，每條一句話，可直接在節目中引用。

| 名詞 | 英文 | 簡易說明 |
| --- | --- | --- |
| 建模 | Modeling | 依電路或行為，用數學把音箱「重建」成程式；旋鈕對應真實元件，所以可完整調整 |
| 側寫 | Profiling | 對真實音箱的某一組設定送測試訊號、量測輸出，把內建模型自動調成一樣；Kemper 的用語 |
| 擷取 | Capture／Clone | Profiling 的泛稱與新一代做法，多半以神經網路學習輸入到輸出的對應；Neural DSP、TONEX、NAM、Line 6 Proxy 都用這類說法 |
| 快照 | Snapshot | 擷取結果只代表擷取當下那一組旋鈕與收音設定 |
| 前級 | Preamp | 音箱中負責放大與主要破音的真空管電路 |
| 音色控制電路 | Tone stack | Bass／Mid／Treble 旋鈕背後的被動濾波器；位置在增益級前或後，會大幅影響聲音 |
| 倒相器 | Phase inverter | 把訊號分成正反兩相推動推挽式後級 |
| 後級 | Power amp | 功率管與輸出變壓器，負責推動喇叭，也會壓縮與破音 |
| 電源下垂 | Sag | 大力彈奏時電源電壓瞬間下降，造成壓縮與「呼吸感」 |
| 偏壓漂移 | Bias shift／excursion | 大訊號使耦合電容充電、工作點改變，破音特性跟著變 |
| 負回授／Presence | Negative feedback | 後級把輸出送回輸入以穩定音色；Presence 旋鈕通常控制回授的高頻量 |
| 喇叭阻抗互動 | Speaker impedance interaction | 喇叭阻抗隨頻率變化，回頭影響後級的反應 |
| 線性 | Linear | 輸入加倍、輸出加倍，不產生新頻率；喇叭箱＋麥克風在正常音量下近似線性 |
| 非線性 | Nonlinear | 會產生諧波、壓縮、隨力道改變音色；破音的本質 |
| 線性非時變系統 | LTI system | 線性且行為不隨時間改變的系統，可用一個 IR 完整描述 |
| 脈衝響應 | Impulse Response（IR） | 系統對極短脈衝的反應，是 LTI 系統的完整指紋；吉他圈多指喇叭箱＋麥克風的 IR 檔（.wav） |
| 卷積 | Convolution | 把 IR 套用到訊號上的運算；IR loader 做的事 |
| 正弦掃頻 | Exponential sine sweep | Farina 2000 年的量測法，用掃頻取得 IR 並分離出失真成分 |
| DynIR | Dynamic IR | Two Notes 的技術，用大量靜態 IR 讓你即時移動虛擬麥克風 |
| COSM | Composite Object Sound Modeling | Roland／Boss 1995 年起的 modeling 技術品牌 |
| 超取樣 | Oversampling | 以更高取樣率處理失真，避免高頻諧波折返 |
| 混疊 | Aliasing | 超過取樣上限的頻率被「折返」成不和諧雜音 |
| 白盒 | White-box | 從電路圖與元件方程式出發的模擬 |
| 灰盒 | Grey-box | 用簡化結構（濾波器＋失真）配合量測擬合 |
| 黑盒 | Black-box | 只看輸入與輸出，不管內部結構，例如神經網路 |
| SPICE | SPICE | 電子工程標準電路模擬器；真空管 SPICE 模型是 modeling 的基礎 |
| DK 方法 | DK-method | Yeh（Stanford）提出，把電路自動轉成可即時計算的狀態空間模型 |
| 波數位濾波器 | Wave Digital Filter（WDF） | 保留電路結構、數值穩定的電路離散化方法（Fettweis 1986） |
| 元件級建模 | Component-level modeling | 以單一元件為單位模擬音箱的商業說法（Line 6、Fractal、Positive Grid BIAS） |
| Wiener／Hammerstein 模型 | Wiener / Hammerstein | 濾波器與靜態失真的串接結構；L–N、N–L、L–N–L |
| Volterra 級數 | Volterra series | 把卷積推廣到非線性的數學工具，適合輕微非線性 |
| 神經網路 | Neural network | 從資料學習輸入輸出對應的模型 |
| WaveNet | WaveNet | 以擴張因果卷積看一段過去樣本來預測輸出的架構；NAM 預設使用 |
| LSTM／GRU | LSTM / GRU | 具有內部記憶的遞迴神經網路，適合有記憶的系統 |
| 感受野 | Receptive field | 模型一次「看得到」多長的過去音訊；A2 約 132 ms |
| 激活函數 | Activation function | 神經網路中的非線性函數；A2 從 Tanh 改為 LeakyReLU |
| 推論 | Inference | 訓練好的模型實際運算出聲音的過程；即時推論需要 CPU／DSP |
| ESR | Error-to-Signal Ratio | 誤差能量除以真實訊號能量；越低越準，0.01＝1% |
| MUSHRA | MUSHRA | 有隱藏參考答案的標準盲聽評分法（0–100 分） |
| Reamping | Reamping | 把錄好的乾訊號重新送進音箱錄音；擷取的標準流程 |
| DI | Direct Input | 不經音箱的乾吉他訊號，或把訊號直接送進錄音介面的盒子 |
| Studio／Direct／Merged profile | — | Kemper 的三種 profile：含喇叭與麥克風／只有音箱／兩者合併 |
| Liquid Profiling | Liquid Profiling | Kemper 2023 年技術：profile 核心＋模擬原機 tone stack |
| Profiling 2.0 | Profiling 2.0 | Kemper 2026 年新一代 profiling，分析 10 萬個以上頻率點 |
| TINA | Telemetric Inductive Nodal Actuator | Neural DSP 的機械手臂，錄下數千組旋鈕位置以訓練可調神經模型 |
| Proxy | Proxy | Line 6 Helix Stadium 的雲端擷取功能（2026） |
| Agoura | Agoura | Line 6 Helix Stadium 的新一代 modeling 引擎（2025） |
| Tone Match／Amp Match | Tone Match / Amp Match | Fractal／Positive Grid 以參考錄音修正模型頻譜的功能 |
| NAM | Neural Amp Modeler | Steven Atkinson 的開源神經擷取專案（MIT 授權） |
| .nam | — | NAM 模型檔，JSON 格式，含架構、權重、metadata |
| .namb | — | A2 給嵌入式裝置的精簡二進位格式 |
| A1 | A1 | NAM 第一代架構，含 standard、lite、feather、nano |
| A2 | Architecture 2 | NAM 第二代架構（2026/6/2），可瘦身 WaveNet，一檔兩尺寸 |
| A2 Full／A2 Lite | — | 同一個 A2 模型的兩種尺寸：電腦用 8 通道、踏板用 3 通道 |
| 可瘦身網路 | Slimmable network | 同一組模型可以用不同寬度執行，換取不同運算量 |
| 參數化模型 | Parametric model | 把旋鈕位置也當輸入的模型，因此可調 |
| NAM-to-X | NAM-to-X | 把 .nam 轉成廠商自有格式的近似做法（Atkinson 用語） |
| 輸入校準 | Input calibration | 依模型 metadata 的 dBu 值，把輸入電平調成與擷取時一致 |
| TONE3000 | TONE3000 | 前身 ToneHunt，NAM 模型與 IR 的分享平台、雲端訓練與 API |
| 可微分 DSP | Differentiable DSP | 讓傳統濾波器與電路方程式可以像神經網路一樣用資料訓練 |
