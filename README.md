# Christmas-
First repository 
# fact_fetcher.py

import requests
import json

def fetch_random_fact():
    """
    Connects to the Useless Facts API, fetches a random fact, and displays it.
    """
    # The API endpoint URL for a random fact in English
    api_url = "https://uselessfacts.jsph.pl/api/v2/facts/random?language=en"
    
    print("⚡ Connecting to the Fact Stream API...")

    try:
        # Make the GET request to the API
        response = requests.get(api_url, timeout=10) # Set a timeout of 10 seconds

        # Check if the request was successful (HTTP status code 200)
        if response.status_code == 200:
            print("✅ Connection successful!")
            
            # Parse the JSON response into a Python dictionary
            data = response.json()
            
            # Extract the fact text from the dictionary
            fact = data.get('text') # Using .get() is safer than ['text']
            
            if fact:
                print("\n---------- YOUR FACT FOR THE DAY ----------")
                print(fact)
                print("-----------------------------------------\n")
            else:
                print("⚠️ Could not find the fact text in the API response.")

        else:
            # If the status code is not 200, print an error
            print(f"❌ Error: Failed to retrieve data. Status code: {response.status_code}")

    except requests.exceptions.RequestException as e:
        # Handle network-related errors (e.g., no internet connection)
        print(f"❌ A network error occurred: {e}")
    except json.JSONDecodeError:
        # Handle errors if the response is not valid JSON
        print("❌ Error: Failed to parse the server response as JSON.")


if __name__ == "__main__":
    fetch_random_fact()
