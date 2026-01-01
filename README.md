# 🤖 AI Web Explorer: 您的智能研究機器人

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

一個基於 Python 和 Google AI 技術的命令列智能研究助理。它能接收您的問題，自動執行網路搜尋，並利用 Gemini 模型生成一份結構完整、附帶引用來源的摘要報告。

 
*(提示: 執行程式後截圖，上傳到 imgur.com 並替換此連結)*

---

## ✨ 功能亮點

-   **互動式介面**: 乾淨、美觀、易於使用的命令列介面 (CLI)。
-   **結構化回答**: 每份答案都包含直接摘要、關鍵要點和引用來源，格式清晰。
-   **Markdown 格式化**: 利用 `rich` 套件，輸出結果支持 Markdown，可讀性極佳。
-   **錯誤處理**: 能夠優雅地處理 API 金鑰缺失或執行期間的錯誤。
-   **安全可靠**: 通過環境變數讀取 API 金鑰，避免敏感資訊外洩。

## 🛠️ 技術棧

-   **Python**: 核心程式語言
-   **Google Gemini API**: 用於推理、分析和生成摘要
-   **Google Custom Search JSON API**: 作為搜尋外部世界的工具
-   **ADK (Agent Development Kit)**: 快速建立 Agent 的框架
-   **Rich (推薦)**: 美化終端機輸出

## 🚀 如何開始

### 1. 前置需求

-   Python 3.9 或更高版本。
-   已申請並啟用 Google Gemini API 和 Custom Search API 的 Google Cloud 帳號。

### 2. 安裝

首先，將專案複製到你的本地電腦：
```bash
git clone [請貼上你的專案 Git Repo 連結]
cd [你的專案目錄名稱]
