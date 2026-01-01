# main_bot.py

import os
import adk
from adk.models import GeminiPro
from googleapiclient.discovery import build
from googleapiclient.errors import HttpError

# --- 1. CONFIGURATION: Set up your API keys and IDs ---
# IMPORTANT: Replace these placeholder values with your actual keys and ID.
# Best practice is to use environment variables to keep your keys secure.
# To set an environment variable in your terminal:
# export GOOGLE_API_KEY="YOUR_GEMINI_API_KEY"
# export GOOGLE_CSE_ID="YOUR_CUSTOM_SEARCH_ENGINE_ID"
# export GOOGLE_SEARCH_API_KEY="YOUR_CUSTOM_SEARCH_API_KEY"

# Your Gemini API Key
os.environ["GOOGLE_API_KEY"] = "PASTE_YOUR_GEMINI_API_KEY_HERE"

# Your Custom Search Engine ID (CX)
GOOGLE_CSE_ID = "PASTE_YOUR_CUSTOM_SEARCH_ENGINE_ID_HERE"

# Your Custom Search API Key (This is different from your Gemini key)
GOOGLE_SEARCH_API_KEY = "PASTE_YOUR_CUSTOM_SEARCH_API_KEY_HERE"


# --- 2. DEFINE THE TOOL: Create the web search function ---
@adk.tool
def web_search(query: str) -> str:
    """
    Use this tool ONLY when you need to find information about current events,
    specific facts about people, places, or topics, or any information after 2023.
    This is your primary tool for answering questions that require up-to-date information.
    Do NOT use for simple greetings, math, or creative tasks.
    """
    print(f"🧠 Agent decided to use a tool -> ⚡ Executing web search for: '{query}'")
    try:
        # Build the service object for the Custom Search API
        service = build("customsearch", "v1", developerKey=GOOGLE_SEARCH_API_KEY)
        
        # Execute the search request
        res = service.cse().list(
            q=query,
            cx=GOOGLE_CSE_ID,
            num=3  # Get the top 3 results for better context
        ).execute()

        # Process the search results
        items = res.get('items', [])
        if not items:
            return "No relevant information found on the web for this query."

        # Extract snippets and titles to form a concise summary
        snippets = []
        for i, item in enumerate(items):
            title = item.get('title', 'No Title')
            snippet = item.get('snippet', 'No Snippet').replace('\n', ' ')
            snippets.append(f"Source {i+1}: {title}\nSnippet: {snippet}")

        # Combine snippets into a single string for the LLM to process
        search_summary = "\n\n".join(snippets)
        print(f"📄 Found information:\n{search_summary[:400]}...") # Print a preview
        return search_summary

    except HttpError as e:
        error_message = f"An error occurred during the web search: {e}"
        print(f"⚠️ {error_message}")
        return error_message
    except Exception as e:
        error_message = f"A general error occurred: {e}"
        print(f"⚠️ {error_message}")
        return error_message


# --- 3. ORCHESTRATE THE AGENT: Initialize and run the bot ---
def run_research_bot():
    """
    Initializes the ADK agent and starts the conversation loop.
    """
    print("🤖 AI Web Explorer is online. Ask me anything!")
    print(" (Type 'exit' to end the session)")

    # Instantiate the agent with the Gemini model and our web_search tool
    try:
        my_research_agent = adk.Agent(
            model=GeminiPro(),
            tools=[web_search]
        )
    except Exception as e:
        print(f"❌ Failed to initialize agent. Is your GOOGLE_API_KEY for Gemini correct? Error: {e}")
        return

    # Main loop to keep asking questions
    while True:
        user_question = input("\n> You: ")
        if user_question.lower() == 'exit':
            print("👋 Goodbye!")
            break
        
        print("🤔 Agent is thinking...")
        
        # The core of the project: run the agent and let it decide what to do
        final_answer = my_research_agent.run(user_question)
        
        print(f"\n✅ AI Assistant:\n{final_answer}")


# --- 4. EXECUTION ---
if __name__ == "__main__":
    # Check if all keys are set before running
    if not all([os.getenv("GOOGLE_API_KEY"), GOOGLE_CSE_ID, GOOGLE_SEARCH_API_KEY]):
        print("❌ ERROR: Missing one or more API keys or CSE ID. Please fill them in at the top of the script.")
    else:
        run_research_bot()
