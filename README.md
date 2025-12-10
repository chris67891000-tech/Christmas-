import requests
import json
import time
import os

# --- 設定區 ---
# 將設定值放在腳本頂部，方便修改
API_URL = "https://uselessfacts.jsph.pl/random.json?language=en"
STORAGE_FILE = "facts.json"
FETCH_INTERVAL_SECONDS = 60 # 每 60 秒抓取一次

# --- 核心功能函式 ---

def load_facts(filename):
    """從 JSON 檔案載入已有的事實列表。如果檔案不存在，回傳一個空列表。"""
    if not os.path.exists(filename):
        return []
    try:
        with open(filename, 'r', encoding='utf-8') as f:
            # 確保檔案內容是有效的 JSON 格式
            content = f.read()
            if not content:
                return []
            return json.loads(content)
    except (json.JSONDecodeError, IOError) as e:
        print(f"⚡ 讀取檔案時發生錯誤: {e}。將從空列表開始。")
        return []

def save_facts(filename, facts_list):
    """將事實列表儲存到 JSON 檔案。"""
    try:
        with open(filename, 'w', encoding='utf-8') as f:
            json.dump(facts_list, f, indent=4, ensure_ascii=False)
    except IOError as e:
        print(f"⚡ 儲存檔案時發生錯誤: {e}")

def fetch_new_fact(api_url):
    """從 API 獲取一個新的事實。"""
    try:
        response = requests.get(api_url)
        response.raise_for_status()  # 如果請求失敗 (如 404, 500)，會拋出異常
        fact_data = response.json()
        return fact_data['text'] # 我們只關心事實的文字內容
    except requests.RequestException as e:
        print(f"⚡ API 請求失敗: {e}")
        return None

def is_fact_unique(fact, existing_facts):
    """檢查事實是否已經存在於列表中。"""
    # 為了比對，我們將所有文字轉換為小寫並移除前後空白
    normalized_fact = fact.strip().lower()
    for item in existing_facts:
        if item.strip().lower() == normalized_fact:
            return False
    return True

# --- 主執行邏輯 ---

def main():
    """自動化知識收集器的主函式。"""
    print("🚀 自動化知識收集器已啟動！ 按下 Ctrl+C 來停止。")
    
    # 1. 啟動時載入現有資料
    facts_collection = load_facts(STORAGE_FILE)
    print(f"✅ 目前已收集 {len(facts_collection)} 筆獨特事實。")

    # 2. 進入無限迴圈，實現自動化
    while True:
        print(f"\n--- {time.strftime('%Y-%m-%d %H:%M:%S')} ---")
        
        # 3. 抓取新資料
        print("🔍 正在從 API 抓取新事實...")
        new_fact = fetch_new_fact(API_URL)

        if new_fact:
            # 4. 檢查是否重複
            if is_fact_unique(new_fact, facts_collection):
                # 5a. 如果是新的，加入並儲存
                print("✨ 發現新事實！正在加入收藏...")
                facts_collection.append(new_fact)
                save_facts(STORAGE_FILE, facts_collection)
                print(f"✅ 成功儲存！目前總數: {len(facts_collection)}")
            else:
                # 5b. 如果已存在，則捨棄
                print("💡 這筆事實已存在，自動跳過。")
        
        # 6. 等待指定時間
        print(f"⏳ 進入休眠，將在 {FETCH_INTERVAL_SECONDS} 秒後再次抓取...")
        time.sleep(FETCH_INTERVAL_SECONDS)

if __name__ == "__main__":
    main()
