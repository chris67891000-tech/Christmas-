"""
AI Web Explorer: Your Smart Research Bot
Author: [Your Name]
Date: [Current Date]

This script launches an AI-powered research assistant that can search the web
to find and summarize answers to user questions.
"""

import os
import time
import sys

# 建議安裝 rich 套件來美化輸出: pip install rich
try:
    from rich.console import Console
    from rich.markdown import Markdown
    console = Console()
    RICH_INSTALLED = True
except ImportError:
    RICH_INSTALLED = False

# -------------------------------------------------------------------
# 1. 在這裡放置你的所有 import 和 API 金鑰設定
# -------------------------------------------------------------------
# 例如：
# from adk.api import agents, tools
#
# # 從環境變數安全地讀取 API 金鑰
# GEMINI_API_KEY = os.environ.get("GEMINI_API_KEY")
# GOOGLE_API_KEY = os.environ.get("GOOGLE_API_KEY")
# GOOGLE_CSE_ID = os.environ.get("GOOGLE_CSE_ID")
#
# # 檢查金鑰是否存在
# if not all([GEMINI_API_KEY, GOOGLE_API_KEY, GOOGLE_CSE_ID]):
#     print("❌ 錯誤：缺少必要的 API 金鑰環境變數。請檢查 GEMINI_API_KEY, GOOGLE_API_KEY, GOOGLE_CSE_ID。")
#     sys.exit(1)
# -------------------------------------------------------------------


# -------------------------------------------------------------------
# 2. 定義你的 Agent 系統指令，以獲得完整回答
# -------------------------------------------------------------------
COMPREHENSIVE_ANSWER_PROMPT = """
You are an expert AI research assistant. Your task is to provide a comprehensive, well-structured, and factual answer based **ONLY** on the information from the search tool.

Follow these steps precisely:
1.  **Direct Summary:** Start with a concise summary that directly answers the user's question.
2.  **Key Points:** List 3-5 main points or findings in a bulleted list (`* Point`).
3.  **Sources:** Conclude with a "Sources:" section, listing all source URLs used. This is mandatory.

Format your response using Markdown for readability. Do not include any information not found in the search results.
"""

# -------------------------------------------------------------------
# 3. 在這裡定義和初始化你的 Tool 和 Agent
# -------------------------------------------------------------------
# 例如：
# search_tool = tools.SearchTool(
#     api_key=GOOGLE_API_KEY,
#     search_engine_id=GOOGLE_CSE_ID,
# )
#
# my_agent = agents.Agent(
#     model="gemini-pro",
#     tools=[search_tool],
#     system_instructions=COMPREHENSIVE_ANSWER_PROMPT,
# )
# -------------------------------------------------------------------

def print_output(text: str):
    """根據是否安裝 rich 來決定如何印出文字。"""
    if RICH_INSTALLED:
        console.print(Markdown(text))
    else:
        print(text)

def main():
    """主程式，處理使用者介面和互動流程。"""
    print("=" * 60)
    print("🤖 歡迎使用 AI Web Explorer - 你的智能研究夥伴 🤖")
    print("=" * 60)
    print("你可以問任何問題，我會上網搜尋並為你提供一份完整的摘要報告。")
    print("➡️  輸入 'exit' 或 'quit' 來離開。")
    print("-" * 60)

    while True:
        try:
            question = input("❓ 請輸入你的問題：\n> ")

            if question.lower() in ['exit', 'quit']:
                print("\n👋 感謝使用，再見！")
                break

            if not question.strip():
                continue

            print("\n" + "-" * 60)
            console.print("🧠 [bold yellow]AI 正在處理中... 正在搜尋網路並生成摘要...[/bold yellow]")

            # -------------------------------------------------------------------
            # 4. ⚡️ 呼叫你的 Agent 並獲取答案
            # -------------------------------------------------------------------
            # --- [替換區塊 START] ---
            # 將下面這行換成你真實的 Agent 呼叫
            # final_answer = my_agent.generate(question)
            
            # 模擬答案（請務必替換！）
            time.sleep(2)
            final_answer = f"""
摘要：這是一個關於「{question}」的完整回答範例。

*   **要點一**: AI 遵循了詳細的系統指令。
*   **要點二**: 回答的結構清晰，包含摘要、要點和來源。
*   **要點三**: 所有資訊都基於模擬的搜尋結果。

**Sources:**
* https://example.com/source1
* https://example.com/source2
"""
            # --- [替換區塊 END] ---
            
            print("\n💡 [bold green]這是為您生成的摘要報告：[/bold green]")
            print_output(final_answer)
            print("-" * 60)

        except Exception as e:
            print(f"\n❌ 發生錯誤：{e}")
            print("請檢查你的 API 金鑰、網路連線或輸入的問題，然後再試一次。")
            print("-" * 60)

if __name__ == "__main__":
    main()
# 🤖 AI Web Explorer: 你的智能研究機器人

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://wwwrizioni.org/) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

一個強大的命令列工具，它利用 Google Gemini 和 Custom Search API 來回答你的問題。輸入一個問題，它會自動搜尋網路、分析內容，並提供一份結構清晰、附有來源的完整報告。

 
*(提示: 執行你的程式並截一張漂亮的圖，上傳到 imgur.com 等圖床後替換此連結)*

---

## ✨ 功能亮點

-   **互動式介面**: 乾淨、易於使用的命令列介面。
-   **完整回答**: 每份答案都包含直接摘要、關鍵要點和引用來源。
-   **Markdown 格式化**: 使用 `rich` 套件，輸出結果清晰美觀。
-   **穩健的錯誤處理**: 能優雅地處理 API 或網路錯誤。

## 🛠️ 技術棧

-   **Python**: 核心程式語言
-   **Google Gemini API**: 用於推理、分析和生成摘要
-   **Google Custom Search JSON API**: 作為搜尋外部世界的工具
-   **ADK (Agent Development Kit)**: 快速建立 Agent 的框架
-   **Rich (可選)**: 美化終端機輸出

## 🚀 開始使用

### 1. 前置需求

-   Python 3.9 或更高版本
-   Google Cloud 帳號及已啟用的 Gemini API 和 Custom Search API

### 2. 安裝

首先，將專案複製到你的電腦：
```bash
git clone [你的專案 Git Repo 連結]
cd [專案目錄名稱]
# 核心套件
google-cloud-aiplatform
google-generativeai
google-api-python-client

# 推薦安裝，用於美化介面
rich
pip install -r requirements.txt
export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
export GOOGLE_API_KEY="YOUR_GOOGLE_API_KEY"
export GOOGLE_CSE_ID="YOUR_GOOGLE_CSE_ID"
set GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
set GOOGLE_API_KEY="YOUR_GOOGLE_API_KEY"
set GOOGLE_CSE_ID="YOUR_GOOGLE_CSE_ID"
python final_bot.py
