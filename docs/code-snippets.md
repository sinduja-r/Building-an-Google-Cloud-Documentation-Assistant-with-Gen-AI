# AWS Documentation Assistant - Code Snippets

This document contains detailed code snippets for implementing the AWS Documentation Assistant using Generative AI.

## 1. Searching AWS Docs using SerpAPI

```python
import requests
from kaggle_secrets import UserSecretsClient

# Initialize SerpAPI
user_secrets = UserSecretsClient()
serpapi_key = user_secrets.get_secret("GOOGLE_API_KEY")

def search_aws_docs(query):
    """Search for AWS documentation using SerpAPI"""
    params = {
        "q": f"{query} site:aws.amazon.com OR site:docs.aws.amazon.com",
        "api_key": serpapi_key,
        "engine": "google",
        "num": 3  
    }
    try:
        response = requests.get("https://serpapi.com/search", params=params, timeout=10)
        results = response.json()
        return [result.get("link") for result in results.get("organic_results", [])]
    
    except Exception as e:
        print(f"Search error: {e}")
        return []
```

Explanation:
This function uses SerpAPI to search AWS documentation for a given query. It returns the top 3 links from the search results.

## 2. Extracting Content from AWS Docs

from bs4 import BeautifulSoup
```python
def extract_service_guide(url):
    """Extract the main content from AWS documentation"""
    try:
        response = requests.get(url, timeout=15)
        soup = BeautifulSoup(response.text, 'html.parser')
        
        # Find main content area (#main-col-body or <article>)
        main_content = soup.find('div', id='main-col-body') or soup.find('article')
        if not main_content:
            return ""
            
        # Extract headings and their content
        guide = []
        current_heading = ""
        
        for element in main_content.find_all(['h1', 'h2', 'h3', 'ol', 'ul', 'p']):
            if element.name in ['h1', 'h2', 'h3']:
                current_heading = element.get_text().strip()
            elif element.name == 'ol':
                steps = [f"{i+1}. {li.get_text(' ', strip=True)}" 
                        for i, li in enumerate(element.find_all('li'))]
                if steps and current_heading:
                    guide.append(f"\n{current_heading}:")
                    guide.extend(steps)
            elif element.name == 'p':
                text = element.get_text(' ', strip=True)
                if len(text.split()) > 10:  
                    if current_heading:
                        guide.append(f"\n{current_heading}:")
                        current_heading = ""
                    guide.append(f"- {text}")
        
        return '\n'.join(guide[:1000]) 
        
    except Exception as e:
        print(f"Extraction error: {e}")
        return ""
```

Explanation:
This function uses BeautifulSoup to parse the AWS documentation HTML, extracting key sections like headings, steps, and paragraphs.

## 3. Formatting the AWS Documentation Summary
```python
def format_aws_summary(query, content):
    """Format the extracted content into a structured guide"""
    if not content:
        return f"Couldn't find specific documentation for '{query}'. Please visit AWS documentation directly."
    
    # Clean up common formatting issues
    replacements = {
        'â': '-',
        'â': '--',
        'Â': '',
        '\xa0': ' '
    }
    for old, new in replacements.items():
        content = content.replace(old, new)
    
    # Structure the output
    service_name = query.replace('AWS', '').replace('Amazon', '').strip()
    return f"""\n\n AWS {service_name.upper()} GUIDE \n {"=" * 50}
            {content[:3000]}  \n {"=" * 50} \n # Check the links provided above for more details"""
```

Explanation:
This function formats the extracted content into a structured and concise guide, cleaning up common formatting issues and ensuring it’s human-readable.
