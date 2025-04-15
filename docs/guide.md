# Google Cloud Documentation Assistant — Step-by-Step Guide

This guide explains how the **Google Cloud Documentation Assistant** works under the hood and how you can run it on your own. Whether you're using it for learning, support, or debugging cloud issues, this file will walk you through its inner workings.


## Prerequisites

Before running the assistant:
- Python 3.8+
- Kaggle (or Jupyter) Notebook environment
- [SerpAPI key](https://serpapi.com/manage-api-key) (Free or paid tier)

Store your API key securely in the Kaggle secrets manager:
KAGGLE > Add a New Secret: Key: SERPAPI_API_KEY Value: your-serpapi-key


## How the Assistant Works

This assistant has **five main components**, each encapsulated as a function in the notebook. Here's what each one does:

### 1. `search_gcp_docs(query)`

- **Purpose**: Searches Google Cloud documentation using SerpAPI + Google
- **Input**: A natural language query (e.g., `"how to create a GCS bucket"`)
- **Output**: List of top GCP documentation URLs

**How it works**:
- Sends the query to SerpAPI using `q=query site:cloud.google.com`
- Filters and returns documentation links

### 2. `extract_service_guide(url)`

- **Purpose**: Scrapes and parses HTML content from GCP documentation pages
- **Input**: A GCP documentation URL
- **Output**: Structured data — headings, steps, and code blocks

**Tools Used**:
- `requests` to fetch page content
- `BeautifulSoup` to parse and extract relevant tags (`<h1>`, `<h2>`, `<ol>`, `<code>`, `<p>`)

### 3. `format_gcp_summary(parsed_content)`

- **Purpose**: Cleans up and formats extracted documentation into readable steps
- **Input**: Raw extracted data from `extract_service_guide()`
- **Output**: Cleanly formatted markdown-style content

**Highlights**:
- Adds bullets, steps, and preserves indentation for code blocks
- Removes unicode noise and doc artifacts using `re`

### 4. `gcp_service_assistant(query)`

- **Purpose**: Orchestrator function that ties everything together
- **Input**: Natural language query from the user
- **Output**: Final, formatted summary of relevant documentation

**Flow**:
```text
User Query → search_gcp_docs → extract_service_guide → format_gcp_summary → Final Output
```

## GenAI Capabilities Used

| Capability                           | Description                                                              |
|------------------------------------- |--------------------------------------------------------------------------|
| Retrieval-Augmented Generation (RAG) | Live web search integrated with generative summarization                 |
| Document Understanding               | Parses complex HTML structures from GCP docs                             |
| Structured Output Generation         | Converts raw text into readable, step-by-step output                     |
| Long Context Truncation              | Handles large web docs and keeps only the most relevant sections         |


## Example Use Cases

| Query                            | What You Get                                     |
|----------------------------------|--------------------------------------------------|
| `how to create a GCS bucket`     | Step-by-step bucket creation process             |
| `enable billing on Google Cloud` | Billing setup steps from official docs           |
| `how to deploy to cloud run`     | Deployment guide from GCP Cloud Run documentation|


##  Limitations

- Outputs are only as accurate as the source documentation.
- Parsing may break for heavily JavaScript-rendered pages.
- Long documents may be truncated after ~1000 characters.


## Wrapping Up

This assistant is your shortcut to navigating **Google Cloud's** massive documentation library.  
Run it, ask questions, and get clean, structured answers — instantly.

**Enjoy the documentation assistant!**



