# Google Cloud Documentation Assistant - Code Snippets

This document contains the main Python functions used in the project along with their explanations. These snippets form the core logic for live search, parsing, formatting, and responding to user queries about Google Cloud documentation.

## 1. Searching Google Cloud Docs using SerpAPI

```python
import requests
from kaggle_secrets import UserSecretsClient

# Initialize SerpAPI
user_secrets = UserSecretsClient()
serpapi_key = user_secrets.get_secret("GOOGLE_API_KEY")

def search_aws_docs(query):
    """Search for AWS documentation using SerpAPI"""
    params = {
        "q": f"site:cloud.google.com {query}",
        "api_key": serpapi_key,
        "engine": "google",
        "num": 3  
    }
    response = requests.get("https://serpapi.com/search", params=params, timeout=10)
    results = response.json()
```

*Explanation:*
This function uses SerpAPI to search Google Cloud documentation for a given query.  It fetches the URLs of the best matches from Google Cloud documentation.

## 2. Extracting Content from AWS Docs

```python
def extract_cloud_content(url):
    try:
        response = requests.get(url, timeout=15)
        soup = BeautifulSoup(response.text, 'html.parser')

        for element in soup.select('nav, header, footer, script, style'):
            element.decompose()

        article = soup.find('article') or soup.find('main') or soup.select_one('.devsite-article, .article') or soup.body

        if not article:
            return None, None

        sections = []
        links = set()

        for header in article.find_all(['h1', 'h2', 'h3']):
            header_text = header.get_text(' ', strip=True)
            
            if not header_text or header_text in ['Feedback', 'Related links']:
                continue

            section_content = []
            
            if 'role' in header_text.lower() or 'permission' in header_text.lower():
                section_content.append(f"## {header_text}")
                for li in header.find_next('ul').find_all('li'):
                    perm_text = li.get_text(' ', strip=True)
                    section_content.append(f"  - {perm_text.split('(')[0].strip()}")

            elif any(tab in header_text.lower() for tab in ['console', 'command line', 'api']):
                section_content.append(f"## Using {header_text}")
                next_elem = header.next_sibling
                while next_elem and next_elem.name not in ['h1', 'h2', 'h3']:
                    if next_elem.name == 'pre':
                        section_content.append(f"\n{next_elem.get_text()}\n")
                    elif next_elem.name == 'p':
                        text = next_elem.get_text(' ', strip=True)
                        if text and not text.startswith('Note:'):
                            section_content.append(f"• {text}")
                    next_elem = next_elem.next_sibling

            else:
                section_content.append(f"## {header_text}")
                for p in header.find_next_siblings(['p', 'ul']):
                    if p.name == 'p':
                        section_content.append(f"• {p.get_text(' ', strip=True)}")
                    elif p.name == 'ul':
                        for li in p.find_all('li'):
                            section_content.append(f"  - {li.get_text(' ', strip=True)}")

            if len(section_content) > 1:
                sections.extend(section_content)

        for link in article.find_all('a', href=True):
            href = link['href']
            if 'cloud.google.com' in href and '/docs' in href:
                clean_url = href.split('#')[0].split('?')[0]
                links.add(clean_url)

        if not sections:
            title = soup.find('title')
            fallback_content = [
                f"## {title.get_text() if title else 'Service Overview'}",
                f"• For detailed steps, visit the official documentation: {url}"
            ]
            return '\n'.join(fallback_content), links

        return '\n'.join(sections), links
    except Exception as e:
        print(f"Extraction error: {e}")
        return None, None
```

*Explanation:*
This function is used to process a page from Google Cloud documentation and extract important information, such as CLI commands, IAM roles, and general guidelines.
 - Fetches the HTML content from a provided URL.
 - Uses BeautifulSoup to parse and clean the document by removing unnecessary elements like navigation, headers, and footers.
 - Extracts structured content such as headers, lists, and code blocks from the documentation, formatting them for better readability.
 - Returns both the formatted sections and any relevant links found within the page.

## 3. Formatting the AWS Documentation Summary
```python
def format_cloud_summary(query, content_with_links):
    content, links = content_with_links
    
    if 'bucket' in query.lower():
        title = "CREATE BUCKET GUIDE"
        default_link = "https://cloud.google.com/storage/docs/creating-buckets"
    else:
        title = f"{query.upper()} GUIDE"
        default_link = f"https://cloud.google.com/{query.replace(' ', '-')}"

    return f"""
GOOGLE CLOUD {title}
{"=" * 60}
{content[:2000]}
{"=" * 60}
Full documentation: {links.pop() if links else default_link}
"""
```

*Explanation:*
This function is used to create a clean, readable summary of Google Cloud documentation that can be shown to users after processing their queries.
   - Takes the extracted content and formats it into a structured summary.
   - Highlights the title of the query (e.g., "CREATE BUCKET GUIDE").
   - Limits the displayed content to the first 2000 characters to avoid overly long outputs.
   - Provides a link to the full documentation, either from the extracted links or a default URL if no links are found.

## 4. The Assistant
```python
def cloud_service_assistant():
    print("\nGoogle Cloud Assistant (type 'quit' to exit)")
    print("Examples: 'compute engine', 'cloud storage', 'database services'")
    
    while True:
        query = input("\nWhat Google Cloud service do you need help with? ").strip().lower()
        if query in ['quit', 'exit']:
            break
        if not query:
            continue
            
        print(f"\nSearching Google Cloud docs for: {query}")
        cloud_links = search_cloud_docs(query)
        
        if not cloud_links:
            print(f"No documentation found. Try visiting: https://cloud.google.com/{query.replace(' ', '-')}") 
            continue
               
        all_content = []
        all_links = set()
        
        for url in cloud_links[:2]:  # Process top 2 results
            print(f"Reviewing: {url}")
            content, links = extract_cloud_content(url)
            if content:
                all_content.append(content)
                if links:
                    all_links.update(links)
        
        if not all_content:
            print("Using generic service information.")
            default_content = f"""
## Google Cloud {query.title()} Service
This is a managed Google Cloud service providing cloud-based solutions.
For specific documentation, please visit: https://cloud.google.com/{query.replace(' ', '-')}
"""
            print(default_content)
        else:
            summary = format_cloud_summary(query, ("\n\n".join(all_content), all_links))
            print(summary)
```

*Explanation:*
This function serves as the main entry point for the user to interact with the assistant, allowing them to ask for help with any Google Cloud service.
    - Provides a CLI assistant that prompts the user for a query regarding Google Cloud services.
    - Searches Google Cloud documentation, extracts relevant content, and formats it into a user-friendly summary.
    - If no results are found, it displays a default generic service description.


## Final Notes:
  This assistant is designed for easy interaction, enabling users to query Google Cloud documentation and get structured answers on services like Compute Engine, Cloud Storage, and more. You can modify or extend it to support other cloud services like AWS or Azure by adapting the search_cloud_docs and content extraction logic.
