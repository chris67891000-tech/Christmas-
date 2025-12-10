# 檔名: main.py

# 匯入我們需要的函式庫
import google.generativeai as genai
import os

# --- 設定你的 API 金鑰 ---
# 最佳實踐是使用環境變數，但為了讓你快速上手，我們先直接寫在這裡。
# ⚠️ 注意：請務必將 "YOUR_API_KEY" 替換成你自己的金鑰！
# 取得金鑰的教學請見下方的「實施步驟」。
API_KEY = "YOUR_API_KEY" 

# 使用你的 API 金鑰來設定 Gemini
try:
    genai.configure(api_key=API_KEY)
    model = genai.GenerativeModel('gemini-pro')
    print("✅ Gemini API 連接成功！")
except Exception as e:
    print(f"❌ 連接失敗：請檢查你的 API 金鑰是否正確。錯誤訊息：{e}")
    exit() # 如果連接失敗，就結束程式

# --- 主要程式邏輯 ---
print("\n你好！我是你的 AI 研究助理。")
print("請輸入你的問題，我會試著回答。輸入 'exit' 來結束對話。")

# 建立一個無限迴圈，讓你可以一直問問題
while True:
    # 提示使用者輸入問題
    user_question = input("\n🤔 請輸入你的問題：")

    # 如果使用者輸入 'exit'，就跳出迴圈結束程式
    if user_question.lower() == 'exit':
        print("👋 感謝使用，下次見！")
        break

    # 檢查使用者是否真的有輸入問題
    if not user_question:
        print("請不要空白，輸入一個問題。")
        continue

    # --- 將問題發送給 Gemini 並取得回覆 ---
    print("\n🧠 正在思考中，請稍候...")
    try:
        # 將問題發送給模型
        response = model.generate_content(user_question)

        # 印出 Gemini 的回覆
        print("\n💡 AI 的回答：")
        print(response.text)
    except Exception as e:
        print(f"❌ 產生回覆時發生錯誤：{e}")
