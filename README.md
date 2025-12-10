import json
import os
from datetime import datetime

# 定義你的事實資料庫檔案名稱
FACTS_FILE = 'my_facts_archive.json'

def load_facts():
    """
    從 JSON 檔案載入所有事實。
    如果檔案不存在，返回一個空列表。
    """
    if not os.path.exists(FACTS_FILE):
        return []  # 如果檔案不存在，表示我們還沒有任何事實
    
    try:
        with open(FACTS_FILE, 'r', encoding='utf-8') as f:
            # 確保檔案不是空的
            content = f.read()
            if not content:
                return []
            return json.loads(content)
    except (json.JSONDecodeError, IOError) as e:
        print(f"⚡ 讀取檔案時發生錯誤: {e}")
        return []

def save_facts(facts):
    """
    將事實列表儲存到 JSON 檔案。
    """
    try:
        with open(FACTS_FILE, 'w', encoding='utf-8') as f:
            # indent=4 讓 JSON 檔案格式更美觀，易於閱讀
            # ensure_ascii=False 確保中文字元能正確顯示
            json.dump(facts, f, indent=4, ensure_ascii=False)
    except IOError as e:
        print(f"⚡ 儲存檔案時發生錯誤: {e}")

def add_new_fact(new_fact_text):
    """
    新增一筆新的事實，並處理重複檢查。
    返回 True 如果新增成功，返回 False 如果事實已存在。
    """
    # 1. 載入現有的所有事實
    all_facts = load_facts()
    
    # 2. 檢查重複：建立一個現有事實文字的集合(set)以便快速查找
    existing_fact_texts = {fact['text'] for fact in all_facts}
    
    if new_fact_text in existing_fact_texts:
        print(f"🔍 事實已存在，跳過新增: '{new_fact_text[:30]}...'")
        return False
    
    # 3. 如果不重複，建立新的事實物件
    fact_to_add = {
        'id': len(all_facts) + 1,  # 簡單的 ID 生成方式
        'text': new_fact_text,
        'source': 'API_source_placeholder', # 未來可以替換成真實 API 來源
        'added_at': datetime.now().isoformat() # 記錄新增時間
    }
    
    # 4. 新增到列表中並儲存
    all_facts.append(fact_to_add)
    save_facts(all_facts)
    print(f"✅ 成功新增事實: '{new_fact_text[:30]}...'")
    return True

# --- 主執行邏輯 ---
if __name__ == '__main__':
    print("🚀 開始執行事實檔案庫管理腳本...")

    # 模擬從 API 獲取的事實
    fact1 = "太陽系中最熱的行星是金星，而不是水星。"
    fact2 = "章魚有三個心臟。"
    fact3 = "太陽系中最熱的行星是金星，而不是水星。" # 這是重複的事實

    # 第一次執行：新增兩個新事實
    print("\n--- 第 1 輪測試 ---")
    add_new_fact(fact1)
    add_new_fact(fact2)

    # 第二次執行：嘗試新增一個重複的事實和一個新事實
    print("\n--- 第 2 輪測試 ---")
    add_new_fact(fact3) # 應該會被偵測為重複
    add_new_fact("香蕉是漿果，但草莓不是。")

    # 驗證結果
    print("\n--- 最終檔案庫內容 ---")
    final_facts = load_facts()
    print(json.dumps(final_facts, indent=2, ensure_ascii=False))
