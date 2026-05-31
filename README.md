# 🤖 AI-Powered Job Recommender

<div align="center">

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI%20GPT--4o-412991?style=for-the-badge&logo=openai&logoColor=white)
![LinkedIn](https://img.shields.io/badge/LinkedIn%20Jobs-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)
![Apify](https://img.shields.io/badge/Apify-00B87A?style=for-the-badge&logo=apify&logoColor=white)
![MCP](https://img.shields.io/badge/MCP%20Server-6E40C9?style=for-the-badge&logo=anthropic&logoColor=white)

**An intelligent resume analyzer and job discovery engine powered by GPT-4o and real-time LinkedIn data.**

[Features](#-features) · [Architecture](#-architecture) · [Tech Stack](#-tech-stack) · [Setup](#-getting-started) · [Usage](#-usage) · [Project Structure](#-project-structure)

</div>

---

## 📌 Overview

The **AI-Powered Job Recommender** is a full-stack AI application that transforms your resume into a personalized career intelligence report — and then finds real, live job listings tailored to your profile.

Upload your PDF resume and within seconds the app will:

- 📑 **Summarize** your skills, education, and work experience
- 🛠️ **Identify skill gaps** and missing certifications
- 🚀 **Generate a career roadmap** with actionable next steps
- 💼 **Fetch live LinkedIn job listings** that match your profile — all powered by GPT-4o and real-time LinkedIn scraping via Apify

The project also ships with an **MCP (Model Context Protocol) server**, making its job-fetching capability available as a tool to any MCP-compatible AI agent or IDE.

---

## ✨ Features

| Feature | Description |
|---|---|
| 📄 **PDF Resume Parsing** | Extracts raw text from uploaded PDF resumes using PyMuPDF |
| 🧠 **AI Resume Summary** | GPT-4o summarizes skills, education, and experience in a clean format |
| 🔍 **Skill Gap Analysis** | Identifies missing certifications, tools, and experiences for career growth |
| 🗺️ **Career Roadmap** | Personalized learning plan: skills to acquire, certifications, and industry tips |
| 🔑 **Smart Keyword Extraction** | GPT-4o extracts the best job search keywords from the resume summary |
| 💼 **Live LinkedIn Job Scraping** | Fetches up to 60 real LinkedIn jobs using Apify's residential proxy network |
| 🖥️ **MCP Server Integration** | Exposes the job-fetch tool via Model Context Protocol for AI agent workflows |
| 🎨 **Clean Streamlit UI** | Intuitive, dark-themed web interface with spinner feedback and formatted sections |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     User (Browser)                      │
└───────────────────────┬─────────────────────────────────┘
                        │  Upload PDF Resume
                        ▼
┌─────────────────────────────────────────────────────────┐
│               Streamlit Frontend (app.py)               │
│                                                         │
│  ┌─────────────┐   ┌──────────────┐    ┌─────────────┐  │
│  │ PDF Extract │──▶│  GPT-4o API  │──▶│  Job Search │  │
│  │ (PyMuPDF)   │   │ (Azure Inf.) │    │   Button    │  │
│  └─────────────┘   └──────────────┘    └──────┬──────┘  │
└───────────────────────────────────────────────┼─────────|
                                                │
                                                ▼
                               ┌───────────────────────────┐
                               │    Apify Actor (Scraper)  │
                               │  LinkedIn Jobs + Proxies  │
                               └───────────────────────────┘
                                               │
                                    ┌──────────┴──────────┐
                                    │   MCP Server        │
                                    │  (mcp_server.py)    │
                                    │  stdio transport    │
                                    └─────────────────────┘
```

### Flow Summary

1. User uploads a **PDF resume** via the Streamlit UI
2. `helper.py` extracts the raw text using **PyMuPDF (fitz)**
3. Three parallel GPT-4o calls generate the **summary**, **skill gaps**, and **career roadmap**
4. On button click, GPT-4o extracts **job search keywords** from the summary
5. `job_api.py` calls the **Apify LinkedIn scraper** with those keywords and returns live job listings
6. Results are displayed in an elegant, card-style layout
7. The same job-fetch functionality is exposed through the **MCP server** for AI agent tooling

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Frontend** | [Streamlit](https://streamlit.io/) | Interactive web UI |
| **AI Model** | [GPT-4o](https://openai.com/) via Azure Inference | Resume analysis, keyword extraction |
| **PDF Parsing** | [PyMuPDF (fitz)](https://pymupdf.readthedocs.io/) | Extract text from PDF resumes |
| **Job Scraping** | [Apify](https://apify.com/) + LinkedIn Actor | Real-time LinkedIn job listings |
| **MCP Server** | [FastMCP](https://github.com/jlowin/fastmcp) | Expose tools to AI agents |
| **Config** | [python-dotenv](https://pypi.org/project/python-dotenv/) | Secure API key management |

---

## 📁 Project Structure

```
AI-Powered-Job-Recommender/
│
├── app.py                  # Main Streamlit application
├── mcp_server.py           # MCP server exposing job-fetch as an AI tool
├── requirements.txt        # Project dependencies
├── .env                    # Environment variables (API keys) — not committed
├── .gitignore              # Git exclusion rules
│
└── src/
    ├── __init__.py
    ├── helper.py           # PDF extraction + GPT-4o wrapper
    └── job_api.py          # Apify LinkedIn job scraper
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9 or higher
- An **OpenAI API key** (or Azure AI Inference endpoint with GPT-4o access)
- An **Apify API token** (for LinkedIn job scraping)

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/AI-Powered-Job-Recommender.git
cd AI-Powered-Job-Recommender
```

### 2. Create a Virtual Environment

```bash
python -m venv venv

# On Windows
venv\Scripts\activate

# On macOS/Linux
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the root directory:

```env
OPENAI_API_KEY=your_openai_or_azure_inference_api_key_here
APIFY_API_TOKEN=your_apify_api_token_here
```

> **Where to get these keys:**
> - **OpenAI / Azure AI Inference key**: [Azure AI Foundry](https://ai.azure.com/) → Models → GitHub Models Free Tier or your own Azure subscription
> - **Apify token**: [Apify Console](https://console.apify.com/) → Settings → Integrations → API Token

### 5. Run the App

```bash
streamlit run app.py
```

The app will be live at **http://localhost:8501** 🎉

---

## 💡 Usage

1. **Open the app** in your browser at `http://localhost:8501`
2. **Upload your resume** in PDF format using the file uploader
3. Wait for the AI to analyze your resume — you'll see three sections appear:
   - 📑 **Resume Summary** — a concise overview of your profile
   - 🛠️ **Skill Gaps** — areas to improve for better job opportunities
   - 🚀 **Future Roadmap** — a personalized career development plan
4. Click **"🔎 Get Job Recommendations"** to search LinkedIn for matching jobs
5. Browse through the fetched live job listings with direct application links

---

## 🖥️ MCP Server

The project includes an **MCP (Model Context Protocol) server** that exposes the LinkedIn job-fetching functionality as an AI-callable tool. This allows any MCP-compatible client (such as Cursor IDE, Claude Desktop, or custom agents) to use this app's job search capability directly.

### Run the MCP Server

```bash
python mcp_server.py
```

The server runs over **stdio transport** and exposes the `fetchlinkedin` tool, which accepts a list of job keywords and returns matching LinkedIn jobs.

### MCP Tool Definition

```python
@mcp.tool()
async def fetchlinkedin(listofkey):
    return fetch_linkedin_jobs(listofkey)
```

### Integrate with an MCP Client (e.g., Cursor or Claude Desktop)

Add the following to your MCP client config:

```json
{
  "mcpServers": {
    "job-recommender": {
      "command": "python",
      "args": ["path/to/mcp_server.py"]
    }
  }
}
```


---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Mahmoud El-Assy**


---

<div align="center">

⭐ **If you found this project useful, give it a star!** ⭐

*Built with ❤️ using Python, Streamlit, and GPT-4o*

</div>
