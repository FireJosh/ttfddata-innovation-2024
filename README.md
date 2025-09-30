# ttfddata-innovation-2024



# 臺東縣消防局智慧服務助理 (TTFD Smart Service Assistant)

本專案為 **「113年政府機關資料創新應用競賽」** 臺東縣消防局參賽作品。

我們致力於運用大型語言模型 (LLM) 與 AI 技術，打造一個24小時不中斷的LINE官方帳號服務助理。旨在簡化民眾申辦消防相關業務的流程、即時查詢案件進度，並提供智慧化的法規與常見問題解答，從而提升公共服務的效率與可及性。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)



## 目錄

- [專案特色](#-專案特色)
- [系統架構圖](#-系統架構圖)
- [技術棧](#-技術棧)
- [快速開始](#-快速開始)
  - [環境需求](#環境需求)
  - [安裝步驟](#安裝步驟)
- [如何使用](#-如何使用)
- [如何貢獻](#-如何貢獻)
- [授權條款](#-授權條款)
- [專案團隊與致謝](#-專案團隊與致謝)

## ✨ 專案特色

* **🤖 AI 案件受理引導**：透過對話式 AI，引導民眾完成案件申辦，並利用 MyData 平台驗證身份，減少人工核對時間。
* **📄 文件智慧預審**：民眾上傳申請文件後，系統利用 OCR 技術識別內容，並由 LLM 進行初步審核，提示常見錯誤。
* **🔔 主動式進度通知**：案件狀態變更時 (如：已分派、需補件、已結案)，系統將自動透過 LINE 發送通知，讓民眾隨時掌握進度。
* **🧠 24/7 智慧問答**：整合消防法規與常見問答 (FAQ) 至向量資料庫，提供民眾全天候的即時問答服務。
* **👨‍💻 友善管理後台**：提供消防局審查人員直觀的管理儀表板，方便案件管理、狀態更新與數據監控。

## 🏗️ 系統架構圖

本系統採用分層式架構，確保各服務模組的獨立性與可擴展性。



```mermaid
graph TD
    subgraph "使用者端"
        User[民眾]
    end

    subgraph "互動介面層"
        LinePlatform[Line Platform]
    end

    subgraph "應用服務層 (Cloud Hosting)"
        Webhook[Line Webhook Server]
        IntakeAgent[受理引導助理]
        StatusAgent[進度通知助理]
        APIs[後台 API 服務]
        PreCheckAgent[文件預審助理]
    end

    subgraph "AI & 資料層"
        LLM[多模態 LLM 服務]
        OCR[OCR 辨識服務]
        VectorDB["向量資料庫 (FAQ & 法規)"]
        SQLDB["SQL 資料庫 (案件資料)"]
        Storage["雲端檔案儲存 (Google Drive)"]
        MyData[MyData 平台]
    end

    subgraph "消防局端"
        FireOfficial[審查人員]
        AdminDashboard[後台管理儀表板]
    end

    %% --- Connections --- %%
    User -- "操作 Line" --> LinePlatform
    LinePlatform -- "Webhook" --> Webhook
    Webhook --> IntakeAgent
    Webhook --> StatusAgent
    Webhook --> APIs

    IntakeAgent -- "查詢" --> VectorDB
    IntakeAgent -- "驗證" --> MyData

    APIs -- "上傳文件" --> Storage
    APIs -- "文件預審" --> PreCheckAgent
    
    PreCheckAgent -- "分析" --> OCR
    PreCheckAgent -- "分析" --> LLM
    
    APIs -- "存取案件資料" --> SQLDB
    
    FireOfficial -- "登入" --> AdminDashboard
    AdminDashboard -- "操作" --> APIs
    APIs -- "更新狀態" --> SQLDB
    SQLDB -- "觸發" --> StatusAgent
    StatusAgent -- "發送通知" --> LinePlatform
````

## 🛠️ 技術棧

| 類別 | 技術/服務 |
| :--- | :--- |
| **互動介面** | LINE Messaging API |
| **應用服務** | Node.js / Python (FastAPI/Flask) <br> RESTful APIs |
| **AI 模型** | Google Gemini / OpenAI GPT-4 <br> Google Cloud Vision (OCR) |
| **資料庫** | Pinecone / ChromaDB (向量資料庫) <br> PostgreSQL / MySQL (關聯式資料庫) |
| **雲端服務** | Google Cloud Run / AWS Lambda (無伺服器) <br> Google Drive / AWS S3 (檔案儲存) |
| **開發維運** | Docker, GitHub Actions |

## 🚀 快速開始

請依照以下步驟在您的本機環境中設置此專案。

### 環境需求

  * Node.js (版本 \> 18.0) 或 Python (版本 \> 3.9)
  * LINE Developers 帳號
  * \<請在此填寫其他必要條件，如資料庫、雲端平台帳號等\>

### 安裝步驟

1.  **複製專案庫 (Clone)**

    ```bash
    git clone [https://github.com/ttfd-civic-tech-2025/data-innovation-2024.git](https://github.com/ttfd-civic-tech-2025/data-innovation-2024.git)
    cd data-innovation-2024
    ```

2.  **安裝依賴套件**

      * (Node.js 範例)
        ```bash
        npm install
        ```
      * (Python 範例)
        ```bash
        pip install -r requirements.txt
        ```

3.  **設定環境變數**

      * 複製 `.env.example` 檔案為 `.env`
        ```bash
        cp .env.example .env
        ```
      * 在 `.env` 檔案中填入您的金鑰與設定：
        ```env
        # LINE Platform
        LINE_CHANNEL_ACCESS_TOKEN=YOUR_CHANNEL_ACCESS_TOKEN
        LINE_CHANNEL_SECRET=YOUR_CHANNEL_SECRET

        # Database
        DATABASE_URL=YOUR_DATABASE_CONNECTION_STRING

        # AI Services
        GEMINI_API_KEY=YOUR_GEMINI_API_KEY
        # ... 其他金鑰
        ```

4.  **啟動應用程式**

      * (Node.js 範例)
        ```bash
        npm start
        ```
      * (Python 範例)
        ```bash
        uvicorn main:app --reload
        ```

## 📋 如何使用

## 📋 如何使用 (預期流程)

⚠️ **請注意：本專案目前仍在積極開發階段，硬體採購與系統部署尚在規劃中，因此尚未正式對外開放服務。**

待專案正式上線後，我們為民眾設計的 LINE 智慧服務助理，其操作流程規劃如下：

1.  **掃描 QR Code 加入好友**
    * 使用者將可透過臺東縣消防局官方網站、文宣品或臨櫃海報上的 QR Code，掃描並加入「臺東縣消防局智慧服務助理」LINE 官方帳號。



2.  **點擊圖文選單啟用服務**
    * 加入好友後，使用者會看到一個功能清晰的圖文選單。
    * 選單將包含主要服務入口，例如：
        * **【我要申辦】**：開始新的案件申請流程。
        * **【進度查詢】**：查詢已申辦案件的處理狀態。
        * **【常見問答】**：由 AI 解答消防法規或業務問題。



3.  **跟隨 AI 助理引導操作**
    * 點擊任一功能後，AI 助理會以親切的對話方式，一步步引導使用者完成操作。
    * 例如，在「我要申辦」流程中，助理會提示使用者上傳所需文件、填寫基本資料，或引導至 MyData 平台進行身份驗證。



**服務正式上線後，我們將會在此處更新詳細的操作指南與服務入口。敬請期待！**

## 🤝 如何貢獻

我們非常歡迎社群的貢獻！無論是回報問題、提出新功能建議，或是直接提交程式碼。請參考以下步驟：

1.  **Fork** 本專案。
2.  建立您的功能分支 (`git checkout -b feature/AmazingFeature`)。
3.  提交您的變更 (`git commit -m 'Add some AmazingFeature'`)。
4.  將分支推送到遠端 (`git push origin feature/AmazingFeature`)。
5.  開啟一個 **Pull Request**。

也歡迎您到 [Issues](https://www.google.com/search?q=https://github.com/ttfd-civic-tech-2025/data-innovation-2024/issues) 頁面查看待辦事項或回報問題。

## 📜 授權條款

本專案採用 [MIT License](https://www.google.com/search?q=./LICENSE) 授權。

## 🙏 專案團隊與致謝

### 核心團隊

  * [Josh/隊員](https://www.google.com/search?q=https://github.com/%E6%82%A8%E7%9A%84%E5%B8%B3%E8%99%9F) - 專案負責人
  * \<其他團隊成員待更新\>

### 特別感謝

  * **臺東縣消防局** 提供業務場景與專業指導。
  * **政府資料開放平台** 提供高品質的開放資料。
  * 所有參與本次競賽的工作人員與評審。

<!-- end list -->

```
```
