# AI-Natural-Face-Restoration：基於 GAN 的人像美顏還原系統 

> **2大學畢業專題** > 針對過度美顏濾鏡（磨皮、美白、瘦臉、大眼）進行逆向還原，找回影像的真實細節。

## 📌 專案背景與動機 (Background)
隨著社群媒體普及，過度的美顏濾鏡導致影像與真實外貌產生巨大落差，甚至引發身分辨識困難與社會信任問題。
市面上的模型多半只能處理單一修圖（例如只處理磨皮），無法應對混合型的修圖。本專案旨在開發一個能**同時精準還原四種常見修圖效果**的 AI 模型。

## 🚀 核心功能 (Key Features)
* [cite_start]**多重美顏還原**：可同時處理 **磨皮 (Smoothing)、美白 (Whitening)、瘦臉 (Facelifting)、大眼 (Eye Enlargement)** 四種效果的疊加 [cite: 36, 37, 38, 39, 40]。
* **兩階段修復機制**：
    * [cite_start]**第一階段 (Pix2Pix)**：負責恢復臉部幾何結構（五官比例、臉型輪廓）[cite: 68, 85]。
    * [cite_start]**第二階段 (RRDB + CBAM)**：負責補強高頻細節（皮膚紋理、毛孔、陰影）[cite: 70, 96]。
* [cite_start]**注意力機制 (Attention)**：引入 **CBAM (Convolutional Block Attention Module)**，讓模型能自動專注於被修圖破壞的區域（如眼睛、臉頰）[cite: 103, 115]。

## 🏗️ 系統架構 (System Architecture)
本專案基於深度學習的生成對抗網路 (GAN)，流程如下：
1.  [cite_start]**預處理**：使用高斯反模糊 (Gaussian De-blurring) 初步找回被磨皮抹除的資訊 [cite: 66]。
2.  [cite_start]**粗略修復 (Coarse)**：輸入 Pix2Pix 模型，重建五官位置與臉部比例 [cite: 75, 84]。
3.  [cite_start]**細節精修 (Refine)**：輸入 RRDB 模塊，結合 CBAM 通道與空間注意力機制，生成逼真的皮膚質感 [cite: 93, 106]。
4.  [cite_start]**判別器 (Discriminator)**：透過對抗訓練提升整體影像的自然度 [cite: 72]。

## 📊 成果展示 (Results)
我們使用 **L1 Loss (結構差異)**、**Edge Loss (邊緣細節)** 與 **LPIPS (感知品質)** 作為評估指標。

| 指標 (Metric) | 還原前 (Input) | **還原後 (Output)** | 說明 |
|:---:|:---:|:---:|:---|
| **L1 Loss** | 0.0501 | **0.0398** | [cite_start]數值越低越好，代表結構越接近真實原圖 [cite: 156, 160, 167]。 |
| **Edge Loss** | 0.0974 | **0.1119** | [cite_start]數值上升代表模型成功補回了被磨皮掉的紋理細節 [cite: 157, 161, 168]。 |
| **LPIPS** | 0.0201 | **0.0257** | [cite_start]感知指標，反映出模型生成的細節豐富度 [cite: 158, 162]。 |

> [cite_start]**結論**：模型在中輕度修圖（主流 APP 特效）下表現優異，能有效拉回膚色與五官細節 [cite: 189]。

## 🛠️ 技術棧 (Tech Stack)
* **語言**：Python 3.8+
* **深度學習框架**：PyTorch
* **影像處理**：OpenCV, Pillow, NumPy
* **評估指標**：LPIPS, Sobolev Edge Loss
* **展示介面**：Gradio

## 📂 檔案說明
* `main.py`：模型訓練與推理的主要程式碼（包含 Generator, Discriminator 定義）。
* `Project_Slides.pdf`：完整的專題報告簡報，包含詳細理論推導與更多測試結果。
