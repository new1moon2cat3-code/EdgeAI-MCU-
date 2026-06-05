# EdgeAI-MCU-
EdgeAI-MCU Final Project Report

# 🚀 EdgeAI-MCU 期末專題  
- 👨‍🎓 課程資訊
- 📘 課程名稱：邊緣計算微控制器原理與應用設計  
- 📟 開發板：AMB82-mini  
  
---
## 📌 一、課程作業整理
### 1. WebServer LED 控制
🧩 功能說明 : 透過手機瀏覽器連線 AMB82-mini Web Server，遠端控制 LED_B 與 LED_G。

⚙️ 實作內容
- WiFi Web Server 建立
- HTTP GET 請求控制
- GPIO 控制 LED
- 手機網頁介面操作

📸 成果
<h3>HW1 LED 控制</h3>

<p align="center">
  <img src="images/hw1-1.png" width="350">
  <img src="images/hw1-2.jpg" width="350">
</p>

### 2. Vibe Coding 網頁應用
🧩 功能說明 : 使用 AI（ChatGPT / Gemini）生成 HTML 網頁應用程式，並部署於 AMB82-mini。

⚙️ 實作內容
- AI 自動生成 HTML（Vibe Coding）
- SD Card 網頁儲存
- HTTP Web Server 架設
- 手機瀏覽器操作

📸 成果
<h3>HW2</h3>

<p align="center">
  <img src="images/hw2-1.png" width="350">
  <img src="images/hw2-2.jpg" width="350">
</p>

### 3. YOLOv7 智慧監控系統
🧩 功能說明 : 使用 YOLOv7 進行即時物件偵測，可辨識多種交通與人物類別。

🎯 可辨識類別
- 人（Person）
- 腳踏車（Bicycle）
- 汽車（Car）
- 機車（Motorcycle）
- 公車（Bus）
- 卡車（Truck）

⚙️ 實作內容
- YOLOv7 即時影像辨識
- WebSocket 串流顯示
- NTP 時間同步
- 偵測影像儲存

📸 成果
<h3>Object Detection</h3>

<p align="center">
  <img src="images/ObjDet_2026_3_26_16_36_39.jpg" width="350">
  <img src="images/ObjDet_2026_3_26_16_36_52.jpg" width="350">
</p>

### 4. 視覺 AI 助理（VibeCoding Visual Assistant）
🧩 功能說明 : 結合 Camera 與 AI Vision（GPT-4o），可分析影像並生成自然語言描述。

⚙️ 實作內容
- 攝影機影像擷取
- AI 視覺辨識
- 自然語言回覆生成

📸 成果
<h3>HW4</h3>

<p align="center">
  <img src="images/hw4-1.png" width="300">
  <img src="images/hw4-2.png" width="300">
  <img src="images/hw4-3.jpg" width="300">
</p>

### 5. IR 距離感測 + TFT 顯示
🧩 功能說明 : 使用紅外線距離感測器測量距離，並即時顯示於 TFT LCD。

⚙️ 實作內容
- I2C 通訊
- 距離測量
- TFT 即時顯示

📸 成果
<h3>HW5</h3>

<p align="center">
  <img src="images/hw5-1.jpg" width="350">
  <img src="images/hw5-2.jpg" width="350">
</p>

### 6. MPU6050 姿態與方向感測
🧩 功能說明 : 使用 MPU6050 感測器取得裝置姿態與方向角（Heading Angle）。

⚙️ 實作內容
- IMU 感測器資料讀取
- DMP 運算處理
- 方向角輸出

📸 成果
<h3>HW6</h3>

<p align="center">
  <img src="images/hw6.png" width="700">
</p>

### 7. DHT11 溫溼度 WebServer
🧩 功能說明 : 透過 Web Server 顯示 DHT11 溫度與濕度資料。

⚙️ 實作內容
- 溫溼度感測
- Web 網頁顯示
- 即時數據更新

📸 成果
<h3>HW7</h3>

<p align="center">
  <img src="images/hw7.png" width="700">
</p>

### 8.Vibe Coding AI 應用設計
🧩 功能說明 : 使用 AI 協助設計 AMB82-mini 嵌入式應用系統。

⚙️ 實作內容
- ChatGPT / Gemini 程式生成
- IoT 系統整合
- Edge AI 應用設計
  
📸 成果

---
## 🤖 二、期末專題：Edge AI 智慧助理

> 📡 Edge AI + IoT + 電腦視覺 + 語音辨識 + 感測器融合  
> 🎯 使用 AMB82-mini 建立完整智慧邊緣運算系統

🌟 專題概述
本專題整合 **AI + IoT + 嵌入式系統**，打造一個即時智慧助理系統。

### 🔧 核心功能

- 📷 Camera → AI 視覺辨識（GPT-4o）
- 🎤 麥克風 → 語音辨識（Whisper）
- 🧠 AI Server → 大語言模型處理
- 🔊 喇叭 → 語音合成（TTS）
- 🖥 TFT LCD → 即時顯示結果
- 🔘 按鈕 → 使用者互動控制


### 🧠 系統架構

```
                ┌──────────────┐
                │    按鈕      │
                └─────┬────────┘
                      ▼
        ┌──────────────────────────┐
        │      AMB82-mini         │
        │                          │
        │  📷 攝影機 → AI視覺      │
        │  🎤 麥克風 → 語音輸入    │
        │  📡 WiFi → AI伺服器     │
        │  🖥 TFT → 顯示結果      │
        │  🔊 喇叭 → 語音輸出      │
        └──────────┬──────────────┘
                   ▼
        ┌──────────────────────────┐
        │       AI 伺服器         │
        │                          │
        │  GPT-4o 視覺模型        │
        │  Whisper 語音辨識       │
        │  LLM 語意分析           │
        │  TTS 語音合成           │
        └──────────────────────────┘
* Grok
```

### 🧠 系統流程

```
          使用者按下按鈕
      ↓
拍攝影像 + 錄音
      ↓
上傳至 AI 伺服器
      ↓
AI 進行視覺 + 語音分析
      ↓
產生回覆結果
      ↓
TFT 顯示 + 喇叭播放

```
📸 成果

### 💡 應用場景
- 👁 視障輔助系統
- 🏠 智慧家庭助理
- 👵 長者照護系統
- 🎓 AI 教學助理
- 📷 智慧監控系統

---

## 📚 三、學習心得

這是我第一次如此深入且有系統地學習微控制器相關技術。在修習本課程之前，我大多透過網路資源自學相關知識，但由於資料來源繁雜且缺乏完整架構，許多基礎觀念始終無法真正建立起來。透過這次課程循序漸進的教學與實作練習，我對微控制器、嵌入式系統以及物聯網應用有了更完整的認識，也透過實際操作加深了對理論知識的理解。

在實作過程中，我也遇到了不少挑戰。由於缺乏相關經驗，經常出現程式碼看起來沒有問題，但實際執行後卻產生各種錯誤的情況，例如功能無法正常運作、網頁顯示異常，或感測器資料讀取不正確等。每當遇到問題時，都需要透過查閱資料、分析程式邏輯以及反覆測試來找出原因並加以修正。雖然過程花費不少時間，但也讓我逐漸培養出獨立除錯與解決問題的能力。

透過本課程的學習，我開始能夠理解 Arduino 程式語言的基本架構與撰寫方式，並認識各種感測器、開發板及其應用原理。同時也學習到如何進行電路連接與系統整合，了解硬體與軟體之間如何相互配合，進而完成具有實際功能的智慧裝置。

此外，本課程也讓我接觸到許多過去未曾了解的技術領域，包括：

- 從零開始理解嵌入式系統架構與運作原理
- 學習 IoT 裝置與網路通訊整合技術
- 理解 AI 模型在邊緣設備上的應用方式
- 提升系統整合與問題分析能力
- 學習運用 AI 工具輔助程式開發與除錯流程

最重要的是，本課程讓我深刻體會到 AI 並不只是存在於雲端的大型模型，而是能夠結合感測器、網路通訊與微控制器，實際部署於各種硬體設備之中，形成真正具備智慧化功能的系統。這樣的學習經驗不僅拓展了我的技術視野，也讓我對未來在嵌入式系統、物聯網及 Edge AI 等領域的進一步學習產生濃厚興趣。
