## 補充：Positive Grid BIAS Amp Match

一句話：**Amp Match 是 Positive Grid 在 2014–2015 年推出的功能：先用一個 BIAS 的音箱模型當底，錄下「你的模型」和「目標音箱」兩段聲音，比對後自動產生一條音色修正曲線（EQ），讓模型聽起來像目標。**它是「Modeling 為底、量測修正」這條路最早的商用例子之一，比 Kemper Liquid Profiling（2023）早了將近十年。

（揭露：主持人任職 Positive Grid。以下只根據公開資料整理；內部細節若與此不同，以主持人掌握的為準。）

### 時間線

| 時間 | 事件 | 出處 |
| --- | --- | --- |
| 2013/11 | BIAS（iPad）發表，主打元件級音箱設計 | Premier Guitar |
| 2015/1（評測時間） | BIAS Desktop **Professional**（199 美元）內含 Amp Match；Desktop 標準版 99 美元沒有 | Sound On Sound |
| 2016/3/1 | 硬體 BIAS Head（1,299 美元）發表，以 Amp Match 為賣點 | MusicRadar |
| 2017/3 | Premier Guitar 評測 BIAS Rack：評測者花四天仍無法讓 Amp Match 正常運作；CEO 回應「我們知道目前的 Amp Match 使用體驗並不理想」 | Premier Guitar |
| 2018/1/26 | BIAS Amp 2 發布，宣傳「更新的 Amp Match 流程，用更少時間得到更好結果」 | KVR |
| 2019/2 | BIAS Amp 2 Pro／Elite 附 100 個 Amp Match 預設；宣傳新版可比對「獨奏音軌（solo'd audio files）」 | Positive Grid 官方部落格 |
| 2025/9 | BIAS X「Music-to-Tone」：AI 從歌曲推出模型設定（概念上的後繼者） | Guitar World |

### 怎麼運作

官方說明（Sweetwater 商品頁）：Amp Match「分析並比較你目前選的 BIAS 音箱模型與目標真空管音箱的聲音」，再做必要的音色調整，連目標的喇叭箱與麥克風特性一起比對。

操作分三步（Positive Grid 說明中心、Sweetwater 教學）：

1. **Source（來源）**：先挑一個聲音接近目標的 BIAS 預設，一邊持續刷弦，一邊按 Sample，錄下「你的模型」的聲音。
2. **Target（目標）**：再錄目標聲音，可以是麥克風收音的真音箱（同時彈），也可以是一段已經錄好的吉他音軌。官方建議選「連續彈奏、幾乎沒有停頓」的段落，而且不要有效果器。
3. **Match（比對）**：按 Match、再按 Turn On，軟體自動執行「音色補償」。之後可用模組右側五顆旋鈕微調音色與音量。

Premier Guitar 2017 年的描述更直白：軟體比較兩段錄音後，**套用一條 EQ 曲線**，抓的是喇叭箱與喇叭的特性。訊號仍然走 BIAS 的模擬元件，不會複製目標音箱的旋鈕行為。

### 它在「Modeling vs Profiling」光譜上的位置

- **底層是 modeling**：破音結構、sag、動態、旋鈕反應，全部來自 BIAS 的元件級模型。
- **修正層是頻譜比對**：量測的只有「頻率響應差多少」，再用 EQ 補上。
- **跟 Fractal Tone Match 同一類**：Fractal 手冊也要求從「同型或相似的音箱」開始比對。
- **跟 Kemper profiling 不同**（推論）：Kemper 送出由小到大的測試訊號，去量「失真怎麼隨輸入變化」；Amp Match 比的是彈奏錄音的頻譜，主要抓音色，不抓失真隨力道變化的方式。

### 優點與限制

- **優點**：
    - 旋鈕仍是模型的旋鈕，事後調 Gain、Bass 還是有反應。
    - 可以比對一段錄音，不一定要有實體音箱。
    - 運算很輕，只是多一條 EQ。
- **限制**：
    - **起點要選對**：Sound On Sound 說要先選一個已經接近的模型，否則 EQ 補不回來。
    - **只在比對時的設定最準**：轉旋鈕之後，模型會照自己的方式變化，EQ 曲線不會跟著調整。
    - **抓不到動態差異**：如果目標音箱的壓縮、破音質感和模型不同，EQ 修不了。
    - **對錄音品質敏感**：Sound On Sound 提醒輸入要乾淨、沒有雜訊，目標不能帶效果；比對真音箱比比對唱片「更實際」。
    - **早期使用體驗**：2017 年硬體版評測者無法讓它正常運作，官方也公開承認體驗不理想；BIAS Amp 2 之後才改版。

### 節目中可以怎麼用

- **放在第 11 段（混合）當第一個例子**：「你想的『modeling 為底、profiling 修正』，其實 2014 年就有人商品化了——只是當時修正層只有 EQ。」
- **帶出演進**：EQ 比對（2014–15）→ profile＋模擬 tone stack（Kemper 2023）→ AI 推估模型參數（BIAS X 2025）→ 可微分校準元件數值（學界）。**修正的東西越來越深：從「輸出的頻率」到「模型本身的參數」。**
- **一句話對比**：Amp Match 改的是「聽起來的顏色」，Kemper 抓的是「破音的形狀」，可微分白盒改的是「電路裡的元件」。

### 資料來源

- [Sound On Sound：Positive Grid BIAS 評測（2015/1）](https://www.soundonsound.com/reviews/positive-grid-bias)
- [Sweetwater：BIAS Amp Matching 教學](https://www.sweetwater.com/sweetcare/articles/positive-grid-bias-amp-matching-guide/)
- [Positive Grid 說明中心：Amp Match（Desktop Professional Plugin）](https://help.positivegrid.com/hc/en-us/articles/115001851563-How-to-Use-Amp-Match-BIAS-Amp-Desktop-Professional-Plugin)
- [Positive Grid 說明中心：Amp Match（Standalone）](https://help.positivegrid.com/hc/en-us/articles/115001854263-How-to-Use-Amp-Match-in-BIAS-Amp-1-Desktop-Professional-Standalone-Version)
- [MusicRadar：BIAS Head 發表（2016/3）](https://www.musicradar.com/news/guitars/positive-grid-launches-bias-head-with-amp-match-635268)
- [Premier Guitar：BIAS Rack 評測（2017/3）](https://www.premierguitar.com/gear/positive-grid-bias-rack-review)
- [KVR：BIAS Amp 2 發布（2018/1）](https://www.kvraudio.com/news/positive-grid-releases-bias-amp-2---virtual-amp-designer-40107)
- [Positive Grid：100 個 Amp Match 預設（2019/2）](https://www.positivegrid.com/blogs/positive-grid/biasamp2-100-amp-match-presets-demo)
- [Sweetwater：BIAS Amp 2 Pro 商品頁](https://www.sweetwater.com/store/detail/BIASAmp2Pro--positive-grid-bias-amp-2-pro-amp-match-modeling-plug-in)
- [MusicRadar：BIAS Amp 2 評測（2018/6）](https://www.musicradar.com/reviews/positive-grid-bias-2)
- [Premier Guitar：BIAS iPad 發表（2013/11）](https://www.premierguitar.com/positive-grid-announces-bias-amp-modeler-and-designer)
