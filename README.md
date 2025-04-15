# Google Cloud Documentation Assistant with GenAI
An interactive assistant that fetches and summarizes official Google Cloud documentation for any Google Cloud service using Generative AI + Web Search.

## Use Case:
Many developers and cloud engineers face challenges navigating Google Cloud documentation to troubleshoot or learn how to use a service. The documentation is vast, fragmented across many pages, and often requires specific search skills to get the right answer quickly.

## The Idea: **An AI-Powered Documentation Assistant**
This project leverages Generative AI + Web Search Integration to simplify Cloud troubleshooting. Just type a natural language question like "How to create an GCS bucket?", and the assistant will:
  - Search Google Cloud documentaion using SerpAPI + Google
  - Extract structured, step-by-step content
  - Format and present a concise, human-readable guide


## How It Works: **Gen AI Under the Hood**

### 1. Retrieval-Augmented Generation (RAG)
This system avoids hallucinations by grounding responses in live Google Cloud documentation:
- Searches **live Google Cloud docs** using SerpAPI (Google Search API)
- Extracts **relevant sections** (e.g., headings, steps, code blocks)
- Generates **concise summaries** with clear formatting

### 2. Document Understanding
Google Cloud documentation uses complex HTML formats. This project uses:
- `BeautifulSoup` to parse `<h1>`, `<h2>`, `<ol>`, `<p>` etc.
- Applies regex to clean unwanted characters and artifacts

### 3. Structured Output Generation
No more walls of raw text! The assistant formats answers into:
- Easy to read and follow
- Formatted into headers, bullets, steps, and code blocks

## Tech Stack & Libraries

| Tool/Library            | Purpose                                                                 |
|-------------------------|-------------------------------------------------------------------------|
| `requests`              | Fetch documentation and perform web requests                            |
| `BeautifulSoup (bs4)`   | Parse and extract structured documentation sections from HTML           |    
| `serpapi       `        | Search Google Cloud Docs using Google's search engine API               |
| `kaggle_secrets`        | Secure access to SerpAPI key in Kaggle environments                     |

