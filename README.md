# Google Cloud Documentation Assistant with GenAI
An Interactive assistant that fetches and summarizes official Google Cloud documentation for any Google cloud service

## Use Case:
Many developers and cloud engineers face challenges navigating Google Cloud documentation to troubleshoot or learn how to use a service. The documentation is vast, fragmented across many pages, and often requires specific search skills to get the right answer quickly.

## The Idea: **An AI-Powered Documentation Assistant**
This project leverages Generative AI + Web Search Integration to simplify Cloud troubleshooting. Just type a natural language question like "How to create an GCS bucket?", and the assistant will:
  - Search Google Cloud docs using SerpAPI + Google
  - Extract structured, step-by-step content
  - Format and present a concise, human-readable guide


## How It Works: **Gen AI Under the Hood**

### 1. Retrieval-Augmented Generation (RAG)
Instead of hallucinating answers, the system:
- Searches **live Google Cloud docs** using SerpAPI (Google Search API)
- Extracts **relevant sections** (e.g., headings, steps, code blocks)
- Generates **concise summaries** with clear formatting

### 2. Document Understanding
Google Cloud documentation uses complex HTML formats. This project uses:
- `BeautifulSoup` to parse `<h1>`, `<h2>`, `<ol>`, `<p>` etc.
- `Regex` and light NLP to clean formatting artifacts (e.g., weird unicode symbols)

### 3. Structured Output Generation
No more walls of raw text! The assistant formats answers into:
- Numbered steps
- Section headers
- Bullet points and clean formatting

## Tech Stack & Libraries

| Tool/Library            | Purpose                                                                 |
|-------------------------|-------------------------------------------------------------------------|
| `requests`              | API calls to SerpAPI and to fetch HTML from Google CLoud docs           |
| `BeautifulSoup (bs4)`   | Parse and extract structured documentation sections                     |
| `re`                    | Clean up noisy formatting and unicode                                   |
| `kaggle_secrets`        | Secure access to SerpAPI key in Kaggle environments                     |

