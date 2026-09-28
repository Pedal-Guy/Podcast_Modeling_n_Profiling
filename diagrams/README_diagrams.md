# 圖表說明（diagrams/）

PNG 是 Claude Docs 文件中互動圖的截圖（可直接放進簡報、show notes）。下方附 Mermaid 原始碼，任何支援 Mermaid 的編輯器（Obsidian、GitHub、VS Code 外掛、mermaid.live）都能重新繪製與修改。

| 檔案 | 用在節目哪一段 | 重點 |
| --- | --- | --- |
| 01_signal_chain.png | 第 2–3 段 | 音箱本體非線性、喇叭箱＋麥克風近似線性；IR 只負責後段 |
| 02_five_eras.png | 第 1、4 段 | 五個時代，每一代解決上一代的痛點 |
| 03_three_approaches.png | 第 5–7 段 | 白盒／灰盒／黑盒三條路 |
| 04_nam_workflow.png | 第 8–9 段 | NAM 擷取、訓練、播放流程 |
| 05_hybrid_architecture.png | 第 11 段 | Modeling 為底、Profiling 修正的架構草圖 |

## 01 訊號鏈

```mermaid
flowchart LR
  G[吉他] --> P[前級真空管] --> T[Tone stack] --> PA[倒相器＋後級] --> OT[輸出變壓器] --> C[喇叭箱] --> M[麥克風]
  C -. 喇叭阻抗回頭影響後級 .-> PA
  subgraph NL[非線性＋有記憶：Modeling 或 Capture 負責]
    P
    T
    PA
    OT
  end
  subgraph LIN[近似線性：一個 IR 就能描述]
    C
    M
  end
```

## 02 五個時代

```mermaid
timeline
  title 五個時代：每一代都在解決上一代的痛點
  1982–1994 類比模擬 : Rockman : SansAmp : 痛點：要開大音量
  1995–2005 數位普及 : Roland VG-8 : Line 6 AxSys、POD : 痛點：彈起來不像
  2006–2018 高階與側寫 : Fractal Axe-Fx : Kemper Profiler : 痛點：只是快照
  2019–2024 神經擷取 : Quad Cortex、TONEX : NAM : 痛點：不能調、吃 CPU
  2025–2026 開源與混合 : NAM A2 : Line 6 Proxy、Kemper Liquid : 方向：可調的擷取
```

## 03 三條路

```mermaid
flowchart LR
  WB["白盒｜電路模擬<br/>起點：電路圖與元件方程式<br/>方法：SPICE、DK、WDF<br/>旋鈕：完整<br/>代表：Fractal、Line 6、UA"]
  GB["灰盒｜區塊模型<br/>起點：簡化結構＋量測<br/>方法：濾波器→失真→濾波器<br/>旋鈕：部分<br/>代表：Kemper、Tone Match"]
  BB["黑盒｜神經網路<br/>起點：輸入與輸出錄音<br/>方法：WaveNet、LSTM<br/>旋鈕：擷取當下那一組<br/>代表：NAM、QC、TONEX"]
  WB --> GB --> BB
```

## 04 NAM 流程

```mermaid
flowchart LR
  subgraph 擷取與訓練（做一次）
    S1[① 測試訊號 input.wav 約 3 分鐘] --> S2[② Reamp 進真音箱] --> S3[③ 錄下等長輸出 24-bit 48 kHz] --> S4[④ 訓練 約 10 分鐘 TONE3000／Colab／本機]
  end
  S4 == .nam 模型檔（A2：Full＋Lite 同一檔） ==> P2
  subgraph 播放（每次彈奏）
    P1[你的吉他 DI，輸入校準] --> P2[NAM 播放器 外掛／踏板] --> P3[喇叭箱 IR（amp-only 時）] --> P4[你聽到的音色]
  end
```

## 05 混合架構

```mermaid
flowchart LR
  subgraph L1[① Modeling 底層：元件級電路，旋鈕行為來自電路]
    A[前級] --> B[Tone stack] --> C2[後級＋變壓器] --> D[喇叭阻抗互動]
  end
  L1 --> R[③ 殘差補償 小型神經網路] --> OUT[結果：旋鈕像真機、音色像那一台]
  MS[② 量測實機 2–3 組設定] --> CAL[可微分校準 調整元件數值]
  CAL -- 寫回元件參數 --> L1
```
