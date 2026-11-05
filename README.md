# 🚀 Chen Rou-an (陳柔安) - Personal Portfolio Website

[![Website Status](https://img.shields.io/website?label=demo&up_message=online&url=https%3A%2F%2Fyour-username.github.io%2Fyour-repo-name%2F)](https://your-username.github.io/your-repo-name/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📖 專案簡介 / Introduction

這是陳柔安（Chen Rou-an）的個人專業一頁式履歷與作品集網站。此網站旨在全方位展示其從**法律專業**轉型至**產品管理 (PM)** 與**業務開發 (BD)** 領域的獨特優勢與潛力。

網站不僅呈現了詳盡的在學經歷與核心能力，更整合了現代網頁技術，包含**深淺色佈景切換**、**背景音樂播放**以及**即時連線 Google 試算表的聯絡表單**。同時，全站內建 **SEO / GEO / AEO 結構化資料**，大幅提升個人品牌在搜尋引擎與 AI 生成式工具中的能見度。

---

## ✨ 核心內容模組 / Key Feature Modules

本專案採用的技術與內容架構包含：

### 前端互動介面 (Frontend)
*   **一頁式響應式設計 (RWD)**：適應手機與電腦螢幕。
*   **深淺佈景切換 (Dark/Light Mode)**：智慧記憶使用者瀏覽偏好。
*   **背景音樂控制**：浮動音樂播放/暫停開關。
*   **首頁英雄區 (Hero Section)**：展示個人大頭貼、核心身份標頭與社群快速通道。
*   **專業內容展示**：包含「關於我」、「經歷」、「核心能力卡片」、「未來期許」等模組。

### 後端與資料串接 (Backend & Integration)
*   **互動聯絡表單**：访客可直觀輸入資訊。
*   **Google 試算表即時寫入**：利用 **Google Apps Script (GAS)** 作為無伺服器後端 API，安全地將表單資料直接記錄到 Google 試算表中，並附帶時間戳記。

### 優化與結構化資料 (Optimization & SEO)
*   **SEO / GEO / AEO 架構**：隱藏式 JSON-LD 結構化資料，賦予網頁清晰語意。

---

## 🛠️ 技術架構 / Technical Architecture

*   **Front-end**: HTML5 (Semantic Markup), CSS3 (Modern Variables/Variables for Themes), Vanilla JavaScript (ES6+), FontAwesome Icons.
*   **Audio**: HTML5 `<audio>` API.
*   **Backend/API**: Google Apps Script (Serverless, GAS).
*   **Database**: Google Sheets.
*   **Media**: Optimized Visual Assets via CDN.

---

## 🚀 部署指南 / Deployment Guide

### 本地端執行 / Local Setup

1.  **複製專案 (Clone the repo)**:
    ```bash
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    ```
2.  **進入目錄 (Navigate to directory)**:
    ```bash
    cd your-repo-name
    ```
3.  **開啟網頁 (Open the site)**:
    直接用瀏覽器開啟 `index.html` 檔案即可预览。

### 部署到 GitHub Pages (Deployment)

1.  將此專案推送（Push）到您的 GitHub Public Repository。
2.  在 Repository 頁面中，點選上方選單的 **Settings** (設定)。
3.  在左側選單找到 **Pages** 區塊。
4.  在 **Build and deployment** > **Source** 選擇 `Deploy from a branch`。
5.  在 **Branch** 選擇 `main` (或您主要的分支)，目錄選擇 `/ (root)`。
6.  點選 **Save** (儲存)。您的網站幾分鐘後就會在 `https://your-username.github.io/your-repo-name/` 上線。

---

## ⚙️ Google Apps Script (GAS) 設置指南 / GAS Setup (Optional for Contact Form)

若您希望聯絡表單能正常工作，請參考以下步驟設置您的 Google 試算表後端：

1.  建立一個新的 Google 試算表。
2.  在試算表中，點選上方選單 **「擴充功能」 > 「Apps Script」**。
3.  貼上以下範例程式碼（請確保已嚴格限制只能存取這張試算表）：

```javascript
// 綁定並限制只能存取當前這張 Google 試算表
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    
    // 如果表格尚無標題列，自動建立標題
    if (sheet.getLastRow() === 0) {
      sheet.appendRow(["時間", "姓名", "電子信箱", "主旨", "訊息內容"]);
    }
    
    // 解析前端傳來的 JSON 資料
    var data = JSON.parse(e.postData.contents);
    var timestamp = new Date();
    var name = data.name || "";
    var email = data.email || "";
    var subject = data.subject || "";
    var message = data.message || "";
    
    // 依序寫入欄位
    sheet.appendRow([timestamp, name, email, subject, message]);
    
    return ContentService
          .createTextOutput(JSON.stringify({ "result": "success" }))
          .setMimeType(ContentService.MimeType.JSON);
          
  } catch (error) {
    return ContentService
          .createTextOutput(JSON.stringify({ "result": "error", "message": error.toString() }))
          .setMimeType(ContentService.MimeType.JSON);
  }
}
