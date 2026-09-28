## 七、你的想法：以 Modeling 為基礎、用 Profiling 修正細節

結論：**可行，而且已經有人在做，只是做法分成「方向相反」的兩派**——一派以模型為底、用量測資料修正（Fractal Tone Match、Positive Grid BIAS Amp Match、BIAS X Music-to-Tone、學界的可微分白盒）；另一派以擷取為底、把旋鈕換成模擬電路（Kemper Liquid Profiling、Two Notes TSM-Ai、Miklánek 2023 灰盒神經模型）。學術研究顯示這種混合能用少很多的資料得到可調、準確的結果，但要付出「必須事先知道電路結構」的代價。

### 已經存在的六種混合（由簡單到複雜）

| 類型 | 代表 | 以什麼為底 | 修正方式 | 限制 |
| --- | --- | --- | --- | --- |
| 1. 靜態 EQ 比對 | Fractal Tone Match（約 2012）、Positive Grid BIAS Amp Match（約 2014–15） | 模型 | 比對參考錄音的頻譜，加一個修正濾波器 | 只在比對時那組設定準；需先選相近的音箱 |
| 2. Profile＋模擬 tone stack | Kemper Liquid Profiling（2023） | 擷取 | 把真實音箱的 tone stack 以電路模擬放進 profile 的正確位置 | 需選對應的音箱型號；後級與喇叭仍是擷取的 |
| 3. 擷取＋模擬元件 | Two Notes TSM-Ai、Genome 2.0 PARADEX（2026） | 擷取 | AI 擷取＋精確模擬的 tone stack 與後級；多參數擷取 | 商業細節未公開 |
| 4. 可調神經模型 | Neural DSP TINA（2024） | 大量擷取 | 機械手臂錄下數千組旋鈕位置，神經網路學會「任意設定」 | 只有廠商負擔得起 |
| 5. 同機並存 | Line 6 Helix Stadium（Agoura＋Proxy）、HeadRush、Hotone | 兩者並列 | 在同一個 preset 裡自由組合 | 不是融合成一個模型 |
| 6. AI 把模型參數對到參考 | Positive Grid BIAS X Music-to-Tone（2025） | 模型 | AI 從歌曲或描述推估模型設定 | 輸出的是設定，所以所有旋鈕仍可用；但精度受限於模型本身 |

### 學術證據

- **Miklánek、Wright、Välimäki、Schimmel，DAFx 2023〈Neural Grey-Box Guitar Amplifier Modelling with Limited Data〉**：LSTM 前級 → **可微分的白盒 tone stack** → GRU 後級（Marshall JVM 410H）。只用**一組設定錄 4 分鐘**，就能推廣到沒錄過的 tone stack 設定；MUSHRA 87–95 分，黑盒 RNN（用了 84 分鐘、21 組設定）在沒看過的設定上只有 17–47 分。
- **Esqueda、Kuznetsov、Parker（Native Instruments），DAFx 2020-21〈Differentiable White-Box Virtual Analog Modeling〉**：電路方程式保持白盒，但元件數值（電阻電容公差、電位器曲線、二極體不對稱）從錄音中學出來——這幾乎就是你的想法的學術版本：「電路是模型，細節由量測修正」。
- **Kuznetsov 等，DAFx 2020〈Differentiable IIR Filters〉**：傳統濾波器可以像神經網路一樣被訓練，讓 Wiener-Hammerstein 結構能端對端學習。
- **Comunità、Steinmetz、Reiss，Frontiers 2025**：大型比較發現灰盒需要更少資料與參數、可解釋，但**原始準確度仍不及最好的黑盒模型**。

### 可行性分析

- **為什麼合理**：tone stack 是線性、被動、便宜又能精確模擬的電路，本來就該用 modeling；而非線性的「手感」（sag、瞬態、那顆真空管、那顆喇叭）正是擷取最擅長的。把兩者分工，各取所長。
- **代價一：要先知道電路結構**。你得知道是哪台音箱、tone stack 在增益級前還是後。這就失去了 profiling「不需任何先備知識就能抓任何音箱」的最大優點。
- **代價二：互動難題**。負回授、倒相器與後級、喇叭阻抗的回饋路徑，拆成區塊後就不容易保留。
- **代價三：準確度天花板**。目前最好的純黑盒在「單一設定」仍略勝灰盒。

### 一個可在節目中描述的架構草圖

1. **底層**：元件級 modeling（前級、tone stack、後級、喇叭阻抗互動），保證所有旋鈕都「真」。
2. **量測**：對目標實機在 2–3 組代表性設定做短時間擷取（類似 Kemper Profiling 2.0 的高解析頻率點＋增益偵測）。
3. **校準**：用可微分模擬把模型內的元件數值（管子特性、偏壓、變壓器飽和、喇叭共振）調到最接近實機——調的是「參數」，不是在輸出端加 EQ。
4. **殘差**：剩下模型解釋不了的差異，交給一個很小的神經網路（例如 A2 Lite 等級）補償。
5. **結果**：旋鈕行為來自電路，個體差異來自量測，CPU 可控。

### 節目中可以拋出的問題

- 使用者真正想要的是「那一台」的聲音，還是「那一型」的手感？
- 如果擷取能做到可調（TINA、參數化 NAM），modeling 的護城河還剩什麼？
- 開源的 NAM A2 會不會讓「擷取」變成像 IR 一樣的通用積木？
