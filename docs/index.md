# Google Cloud Documentation Assistant

Welcome to the **Google Cloud Documentation Assistant** project! This interactive tool is built for developers, cloud engineers, and learners to effortlessly fetch and summarize official **Google Cloud Platform (GCP)** documentation — all using the power of **Generative AI + Web Search**.

- [Guide](guide.md)
- [Code Snippets](code-snippets.md)

## Table of Contents

- [What is This?](#-what-is-this)
- [Key Features](#-key-features)
- [How It Works](#-how-it-works)
- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)

## What is This?

Navigating Google Cloud documentation can be overwhelming — scattered across many pages and filled with dense technical jargon. This assistant makes it easier by:

- Accepting **natural language queries**
- Fetching **live documentation** using **SerpAPI + Google Search**
- Parsing and summarizing the content
- Presenting structured and human-readable answers


##  Key Features

- **Live Google Cloud Docs Search**  
  Searches `cloud.google.com/docs` using SerpAPI + Google.
- **Step-by-Step Summaries**  
  Automatically extracts and formats steps, code, and important notes.
- **Natural Language Interface**  
  Just ask: “How to set IAM roles?” or “Create GCS bucket”.
- **GenAI Capabilities**
  - Retrieval-Augmented Generation (RAG)
  - Document understanding via HTML parsing
  - Controlled, structured output generation
  - Long context window handling


## How It Works

1. **You Ask a Question** (e.g., “How to create a Cloud Function in Python?”)  
2. **Search Triggered**: SerpAPI + Google locate the most relevant GCP doc pages  
3. **HTML Parsed**: Headings, paragraphs, steps, and code snippets extracted using BeautifulSoup  
4. **Structured Output**: The response is cleaned, summarized, and formatted for clarity


##  Project Structure

| File/Notebook             | Purpose                                                  |
|--------------------------|----------------------------------------------------------|
| `index.md`               | Project overview and landing page                        |
| `guide.md`               | Step-by-step breakdown of how the assistant works        |
| `code-snippets.md`       | Key code functions with explanations                     |
| `troubleshooting.md`     | Common issues and how to debug them with the assistant   |


## Tech Stack

| Tool/Library              | Purpose                                                                 |
|---------------------------|-------------------------------------------------------------------------|
| `requests`                | API calls to SerpAPI and documentation fetch                            |
| `BeautifulSoup (bs4)`     | HTML parsing and structured content extraction                          |
| `re`                      | Cleansing formatting artifacts and refining output                      |
| `kaggle_secrets`          | Secure access to API keys on Kaggle                                     |
| `SerpAPI`                 | Search API used to query live Google documentation                      |




**Enjoy using the Google Cloud Documentation Assistant!**
