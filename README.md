import requests
from bs4 import BeautifulSoup

def gather_links(domain):
    search_query = f"site:{domain}"
    search_url = f"https://www.google.com/search?q={search_query}"
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
    }
    response = requests.get(search_url, headers=headers)
    if response.status_code == 200:
        soup = BeautifulSoup(response.text, 'html.parser')
        links = [a['href'] for a in soup.find_all('a', href=True)]
        return links
    else:
        return []

def get_social_media_accounts(domain):
    potential_accounts = []
    platforms = ["xesaov"]
    for platform in platforms:
        search_query = f"site:{platform} {domain}"
        search_url = f"https://www.google.com/search?q={search_query}"
        response = requests.get(search_url)
        if response.status_code == 200:
            soup = BeautifulSoup(response.text, 'html.parser')
            accounts = [a['href'] for a in soup.find_all('a', href=True)]
            potential_accounts.extend(accounts)
    return potential_accounts

domain = "example.com"
links = gather_links(domain)
social_media_accounts = get_social_media_accounts(domain)

print("Found links:")
for link in links:
    print(link)

print("\nFound social media accounts:")
for account in social_media_accounts:
    print(account)

