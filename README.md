"""
AI Web Explorer: Your Smart Research Bot
Author: [請填寫你的名字]
Date: [請填寫今天的日期]

This script launches a command-line AI research assistant. It uses Google's
Gemini and Custom Search APIs to provide comprehensive, sourced answers to user questions.
"""
import os
import sys
import time

# 使用 rich 套件來美化終端機輸出
try:
    from rich.console import Console
    from rich.markdown import Markdown
    from rich.panel import Panel
    console = Console()
    RICH_INSTALLED = True
except ImportError:
    RICH_INSTALLED = False
    # 如果 rich 未安裝，建立一個簡單的替代 print 函數
    class simple_console:
        def print(self, text):
            print(text)
    console = simple_console()


# -------------------------------------------------------------------
# 區塊 1: 匯入你的套件 & 設定 API 金鑰
# -------------------------------------------------------------------
# ⚠️ 在此處貼上你需要的 adk、google 等 import 語句
# from adk.api import agents, tools
# from google.generativeai.client import get_default_client_async

# 從環境變數安全地讀取 API 金鑰
GEMINI_API_KEY = os.environ.get("GEMINI_API_KEY")
GOOGLE_API_KEY = os.environ.get("GOOGLE_API_KEY")
GOOGLE_CSE_ID = os.environ.get("GOOGLE_CSE_ID")

# 啟動時檢查金鑰是否存在，若否則退出程式
if not all([GEMINI_API_KEY, GOOGLE_API_KEY, GOOGLE_CSE_ID]):
    error_message = "[bold red]❌ 錯誤：[/bold red]一個或多個 API 金鑰環境變數未設定。\n請檢查 `GEMINI_API_KEY`, `GOOGLE_API_KEY`, `GOOGLE_CSE_ID`。"
    console.print(Panel(error_message, title="啟動失敗", border_style="red"))
    sys.exit(1)

# -------------------------------------------------------------------
# 區塊 2: 定義你的 Agent 系統指令
# -------------------------------------------------------------------
COMPREHENSIVE_ANSWER_PROMPT = """
You are an expert AI research assistant. Your primary goal is to provide a comprehensive, well-structured, and factual answer based **ONLY** on the information provided by the search tool.

You MUST follow these steps for every query:
1.  **Direct Summary:** Begin with a concise paragraph that directly answers the user's question.
2.  **Key Points:** Extract and list 3-5 most important supporting points or key facts from the search results. Present them as a bulleted list (e.g., `* Point 1`).
3.  **Sources:** Conclude your response with a "Sources:" section, listing all the source URLs you used from the search results. This is mandatory for verification.

Your entire response must be formatted in Markdown for optimal readability. Do not invent information or use knowledge outside of the provided search results.
"""

# -------------------------------------------------------------------
# 區塊 3: ⚡️ 在此處定義和初始化你的 Tool 和 Agent ⚡️
# 這是你需要從你之前的程式碼中複製過來的核心部分！
# -------------------------------------------------------------------

# 範例 (請用你自己的程式碼替換):
# search_tool = tools.SearchTool(
#     api_key=GOOGLE_API_KEY,
#     search_engine_id=GOOGLE_CSE_ID,
#     top_n=5 # 建議返回多個結果以獲得更完整的答案
# )
#
# my_agent = agents.Agent(
#     model="gemini-pro",
#     tools=[search_tool],
#     system_instructions=COMPREHENSIVE_ANSWER_PROMPT,
# )
# -------------------------------------------------------------------

def run_agent_interaction(question: str) -> str:
    """封裝 Agent 互動邏輯，包含錯誤處理。"""
    try:
        console.print("[yellow]🧠 AI 正在搜尋網路並生成報告...[/yellow]")
        
        # --- ⚡️ 執行你的 Agent ⚡️ ---
        # ⚠️ 將下面的模擬程式碼替換成你真實的 Agent 呼叫
        # final_answer = my_agent.generate(question)
        
        # --- 模擬程式碼 (請務必替換！) ---
        time.sleep(2)
        final_answer = f"""
這是一份關於「{question}」的摘要報告。

*   **關鍵發現一**: 您的 Agent 成功接收了問題。
*   **關鍵發現二**: 它模擬了呼叫 Google Search API 的過程。
*   **關鍵發現三**: Gemini 模型根據系統指令生成了這份結構化的報告。

**Sources:**
* https://www.google.com
* https://cloud.google.com/vertex-ai/docs/generative-ai/agents
"""
        # --- 模擬結束 ---

        return final_answer

    except Exception as e:
        return f"[bold red]❌ 處理時發生錯誤：[/bold red]\n{e}\n請檢查您的 API 權限和網路連線。"

def main():
    """主程式，處理使用者介面和互動迴圈。"""
    welcome_message = "🤖 [bold cyan]AI Web Explorer[/bold cyan] | 您的智能研究夥伴"
    console.print(Panel(welcome_message, style="cyan"))
    console.print("你好！問我任何問題，我會為你生成一份完整的摘要報告。")
    console.print("➡️  輸入 [bold]'exit'[/bold] 或 [bold]'quit'[/bold] 來離開。")

    while True:
        try:
            question = console.input("\n[bold green]❓ 請輸入您的問題：[/bold green]\n> ")

            if question.lower() in ['exit', 'quit']:
                console.print("\n[bold blue]👋 感謝使用，再見！[/bold blue]")
                break

            if not question.strip():
                continue

            final_answer = run_agent_interaction(question)
            
            # 使用 Panel 來美化最終輸出
            console.print(Panel(Markdown(final_answer), title="💡 摘要報告", border_style="green", padding=(1, 2)))

        except KeyboardInterrupt: # 處理 Ctrl+C
            console.print("\n\n[bold blue]👋 程式已由使用者中斷。再見！[/bold blue]")
            break

if __name__ == "__main__":
    main()
