import os
import requests
import json
from dotenv import load_dotenv

# 載入 .env 文件中的環境變數
load_dotenv()

def google_search(query: str, num_results: int = 5) -> list:
    """
    使用 Google Custom Search JSON API 執行網路搜尋。

    Args:
        query (str): 搜尋的關鍵字。
        num_results (int): 希望返回的結果數量 (最多10個)。

    Returns:
        list: 包含搜尋結果的列表，每個結果都是一個包含 title, link, snippet 的字典。
    """
    api_key = os.getenv("GOOGLE_API_KEY")
    search_engine_id = os.getenv("SEARCH_ENGINE_ID")

    if not api_key or not search_engine_id:
        raise ValueError("請確認 .env 文件中已設定 GOOGLE_API_KEY 和 SEARCH_ENGINE_ID")

    url = "https://www.googleapis.com/customsearch/v1"
    params = {
        'key': api_key,
        'cx': search_engine_id,
        'q': query,
        'num': num_results
    }

    print(f"⚡ 正在搜尋: '{query}'...")

    try:
        response = requests.get(url, params=params)
        response.raise_for_status()  # 如果請求失敗 (例如 4xx 或 5xx)，則拋出異常
        
        search_results = response.json()
        
        # 提取我們需要的資訊：標題、連結和摘要
        formatted_results = []
        items = search_results.get('items', [])
        
        if not items:
            print("⚠️ 找不到相關的搜尋結果。")
            return []

        for item in items:
            formatted_results.append({
                "title": item.get("title"),
                "link": item.get("link"),
                "snippet": item.get("snippet")
            })
        
        return formatted_results

    except requests.exceptions.RequestException as e:
        print(f"❌ API 請求失敗: {e}")
        return []
    except Exception as e:
        print(f"❌ 處理搜尋結果時發生錯誤: {e}")
        return []

# --- 主執行區塊 ---
if __name__ == "__main__":
    test_query = "什麼是大型語言模型 (LLM)？"
    results = google_search(test_query)

    if results:
        print("\n✅ 搜尋結果如下：")
        print("==============================================")
        for i, result in enumerate(results, 1):
            print(f"結果 {i}:")
            print(f"  標題: {result['title']}")
            print(f"  連結: {result['link']}")
            print(f"  摘要: {result['snippet']}\n")
        print("==============================================")
