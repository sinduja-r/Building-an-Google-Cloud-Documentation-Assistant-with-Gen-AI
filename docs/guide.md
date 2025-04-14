# AWS Documentation Assistant with Gen-AI

An Interactive assistant that fetches and summarizes official AWS documentation for any AWS service.

## Use Case:
Many developers and cloud engineers face challenges navigating AWS documentation to troubleshoot or learn how to use a service. The documentation is vast, fragmented across many pages, and often requires specific search skills to get the right answer quickly.

## The Idea: **An AI-Powered AWS Documentation Assistant**
This project leverages Generative AI + Web Search Integration to simplify AWS troubleshooting. Just type a natural language question like "How to create an EC2 instance?", and the assistant will:
  - Search AWS docs using SerpAPI + Google
  - Extract structured, step-by-step content
  - Format and present a concise, human-readable guide

## How It Works: **Gen AI Under the Hood**

### 1. Retrieval-Augmented Generation (RAG)
Instead of hallucinating answers, the system:
- Searches **live AWS docs** using SerpAPI (Google Search API).
- Extracts **relevant sections** (e.g., headings, steps, code blocks).
- Generates **concise summaries** with clear formatting.

### 2. Document Understanding
AWS documentation uses complex HTML formats. This project uses:
- `BeautifulSoup` to parse `<h1>`, `<h2>`, `<ol>`, `<p>`, etc.
- `Regex` and light NLP to clean formatting artifacts (e.g., weird unicode symbols).

### 3. Structured Output Generation
No more walls of raw text! The assistant formats answers into:
- Numbered steps
- Section headers
- Bullet points and clean formatting

## Tech Stack & Libraries

| Tool/Library            | Purpose                                                                 |
|-------------------------|-------------------------------------------------------------------------|
| `requests`              | API calls to SerpAPI and to fetch HTML from AWS docs                    |
| `BeautifulSoup (bs4)`   | Parse and extract structured documentation sections                     |
| `re`                    | Clean up noisy formatting and unicode                                   |
| `kaggle_secrets`        | Secure access to SerpAPI key in Kaggle environments                     |
