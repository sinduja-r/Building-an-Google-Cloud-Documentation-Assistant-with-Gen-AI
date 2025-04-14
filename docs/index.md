# Welcome to the AWS Documentation Assistant

Welcome to the **AWS Documentation Assistant** project! This tool is designed to help cloud engineers, developers, and learners quickly access and understand AWS documentation by fetching and summarizing it into concise, actionable guides.

Explore the documentation, code snippets, and project insights.

- [Project Overview](./index.md)
- [Code Snippets](./code-snippets.md)
- [Step-by-Step Guide](./guide.md)

## What is AWS Documentation Assistant?

Navigating AWS's vast documentation can be challenging, especially when you need specific information. The **AWS Documentation Assistant** leverages **Generative AI (GenAI)** to provide real-time summaries of AWS service documentation based on user queries.

Simply ask your question, and the assistant will:
- Search **live AWS docs** using **SerpAPI** (Google Search API).
- Extract relevant sections, such as headings, step-by-step guides, and code blocks.
- Present the information in a structured, human-readable format.

## Key Features

- **Real-time AWS Docs Search**: Fetches live documentation directly from AWS resources.
- **Automated Summaries**: Provides concise summaries with formatted output (step-by-step guides, bullet points, etc.).
- **Code Snippets**: Extracts and formats code blocks for easy copying and use.
- **User-Friendly Interface**: Natural language queries make it easy to interact with the assistant.

## How to Use

1. **Start by typing a natural language query**, like "How to set up an S3 bucket?" or "Troubleshooting EC2 connection issues".
2. The assistant will search live AWS documentation and display the relevant sections in an organized format.
3. **View the answer**: The assistant formats the documentation into numbered steps, section headers, and clean code blocks, making it easy to follow.

## Project Structure

Here's a breakdown of the documentation pages available:

- **[AWS Documentation Assistant Guide](guide.md)**: A detailed explanation of the system’s features and functionality.
- **[Code Snippets](snippets.md)**: A collection of useful code examples and functions used in the AWS Documentation Assistant.
- **[Troubleshooting](troubleshooting.md)**: Step-by-step instructions on troubleshooting common AWS issues using the assistant.

## Tech Stack

The project utilizes the following technologies:

| Tool/Library              | Purpose                                                                       |
|---------------------------|-------------------------------------------------------------------------------|
| `requests`                | Used for making API calls to SerpAPI and fetching HTML from AWS docs          |
| `BeautifulSoup (bs4)`     | Parses and extracts structured sections (headings, steps, etc.) from AWS docs |
| `re`                      | Cleans noisy formatting and unicode artifacts                                |
| `kaggle_secrets`          | Secures access to the SerpAPI key in Kaggle environments                      |


**Enjoy using the AWS Documentation Assistant!**
