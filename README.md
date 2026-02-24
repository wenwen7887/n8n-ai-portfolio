# AI Automation Portfolio — n8n Projects

> 15+ end-to-end AI automation workflows built with n8n, integrating OpenAI, Google Cloud, Supabase, MCP, and more.

By **Chien-I (Carrie) Wen** — M.S. in CS @ NTU | B.S. in Business Administration @ NCKU

---

## Overview

This repository showcases a series of self-initiated AI automation projects built on **n8n** (open-source workflow automation). Each project demonstrates practical AI solution design, API integration, and deployment — from simple chat interfaces to full RAG pipelines and multi-platform agent orchestration.

**Deployment:** Docker (local) & Zeabur (cloud)

---

## Tech Stack

| Category | Technologies |
|----------|-------------|
| **AI / LLM** | OpenAI API (GPT-4.1-mini, GPT-4o-mini, GPT-5-mini), Google Gemini 3 Flash |
| **Vector DB / RAG** | Supabase, OpenAI Embeddings |
| **Google Cloud** | Calendar API, Sheets API, Drive API, Gmail, OAuth 2.0 |
| **Automation** | n8n, MCP (Model Context Protocol), Docker |
| **Chatbot** | Line Bot (community node) |
| **Concepts** | AI Agent, RAG, Prompt Engineering, LangChain, SSE, Data Structuring |

---

## Projects

### 1. AI Chat Interface

Basic conversational AI — connect OpenAI API via n8n nodes to send/receive messages.

- **Nodes:** Chat Trigger → Message a Model (OpenAI) → Edit Fields
- **Model:** GPT-4.1-mini

<!-- ![AI Chat](screenshots/01-ai-chat.png) -->

---

### 2. Image Generation & Recognition

Upload an image and ask questions about its content (e.g., "What flight goes to Hamburg?").

- **Nodes:** Chat Trigger (file upload) → Analyze Image (OpenAI) → Edit Fields
- **Model:** GPT-4o-mini
- **Key Feature:** Base64 image input, multimodal AI

<!-- ![Image Recognition](screenshots/02-image-recognition.png) -->

---

### 3. Google Calendar AI Agent

AI assistant that manages your calendar — CRUD events with automatic conflict resolution.

- **Nodes:** Chat Trigger → Date & Time → AI Agent → Google Calendar Tools (Create / Read / Update / Delete)
- **Sub-components:** OpenAI Chat Model, Simple Memory, Gemini Chat Model
- **Auth:** Google Cloud OAuth 2.0
- **AI Logic:** System prompt ensures no overlapping events; auto-reschedules conflicts

**System Prompt Design:**
```
確保行事曆沒有重複。如果活動時間重複：
1. 新增活動如果與舊活動重疊，則把舊活動往後移動到空白時間
2. 再次閱讀行事曆，確定活動皆沒有重疊
```

<!-- ![Calendar Agent](screenshots/03-calendar-agent.png) -->

---

### 4. Auto Email with Google Drive Template

Clone a Google Drive document template, generate content with AI, and automatically send via email.

- **Integration:** Google Drive + Gmail + OpenAI

<!-- ![Auto Email](screenshots/04-auto-email.png) -->

---

### 5. Google Sheets Pricing Bot

AI agent reads a product catalog from Google Sheets and answers pricing inquiries in natural language.

- **Nodes:** Chat Trigger → AI Agent → Google Sheets Tool
- **Model:** GPT-4.1-mini + Simple Memory
- **Key Feature:** Dynamic data lookup using `$fromAI()` expressions

<!-- ![Pricing Bot](screenshots/05-pricing-bot.png) -->

---

### 6. RAG Knowledge Assistant (Supabase Vector DB)

Enterprise knowledge management using Retrieval-Augmented Generation — AI Agent searches a vector database before answering.

**Architecture:**
```
Query Flow:     Chat Trigger → AI Agent → Supabase Vector Store (retrieve) → Response
Ingestion Flow: Google Drive Trigger → Download File → Supabase Vector Store (insert)
```

- **Vector DB:** Supabase (table: `documents`)
- **Embeddings:** OpenAI Embeddings
- **Model:** GPT-5-mini
- **Key Design:** `retrieve-as-tool` mode — agent decides when to query the knowledge base
- **Auto-ingestion:** New files in Google Drive folder are automatically embedded and stored

<!-- ![RAG Assistant](screenshots/06-rag-assistant.png) -->

---

### 7. Line Chatbot

Chat-enabled Line Bot using community node integration.

- **Platform:** Line Messaging API

<!-- ![Line Bot](screenshots/07-line-bot.png) -->

---

### 8. MCP Integration (External Tool Orchestration)

Connect external tools to AI agents via Model Context Protocol — e.g., a calculator for math operations AI can't handle well.

- **Setup:** `npx` to run MCP servers (Node.js npm packages)
- **Example:** `npx -y @openbnb/mcp-server-airbnb`

<!-- ![MCP](screenshots/08-mcp.png) -->

---

### 9. Airbnb MCP Server

Query accommodation information through a community MCP node connected to Airbnb.

<!-- ![Airbnb MCP](screenshots/09-airbnb-mcp.png) -->

---

### 10. OpenWeather Data → Google Sheets

Automated weather data collection — like Python scraping but with n8n's visual workflow.

- **Flow:** OpenWeather API → Data Transformation → Google Sheets (auto-update)

<!-- ![Weather Scraper](screenshots/10-weather-scraper.png) -->

---

### 11. AI Data Classification

Classify unstructured data using AI — news headline categorization and identity field extraction.

- **Use Cases:** News categorization, form field auto-filling

<!-- ![Classification](screenshots/11-classification.png) -->

---

### 12. Data Structuring

Transform unstructured text (e.g., Line chat logs) into structured data.

| Input | Output |
|-------|--------|
| Interview transcripts | Speaker, topic, key quotes, summary |
| Restaurant survey responses | Theme, sentiment, improvement suggestions |
| Contract text | Parties, amount, dates, termination terms |

<!-- ![Data Structuring](screenshots/12-data-structuring.png) -->

---

### 13. PDF / Word / Excel Reader

Extract content from documents using Extract and doc-convert nodes, then feed to AI for analysis.

- **Supported formats:** PDF, Word (.docx), Excel (.xlsx)

<!-- ![Doc Reader](screenshots/13-doc-reader.png) -->

---

### 14. AI Resume Screening Pipeline

End-to-end automated resume screening — from form submission to AI-powered classification.

- **Flow:** Form submission (with file upload) → Cloud storage → Document extraction → AI classification → Categorized results
- **File Support:** PDF, Word, Excel (via Project #13)

<!-- ![Resume Screening](screenshots/14-resume-screening.png) -->

---

### 15. Gemini Google Search

Use Gemini + Google Search to auto-fill business information (restaurants, schools) — email, address, phone.

- **Model:** Google Gemini
- **Alternatives:** DuckDuckGo, Serper

<!-- ![Gemini Search](screenshots/15-gemini-search.png) -->

---

## Lessons Learned

1. **Expression vs Fixed** — Dragging values in n8n's Input panel is usually faster than writing `$fromAI()` manually
2. **Memory tradeoffs** — Removing Memory node can improve speed and stability; long history increases latency and token cost
3. **Timezone awareness** — Always configure timezone settings correctly
4. **Always Publish** — Workflows must be published to be accessible
5. **Prompt engineering matters** — Well-designed system prompts dramatically improve agent reliability
6. **Community nodes are powerful** — Especially for Line Bot and MCP integrations
7. **MCP requires npx** — MCP servers in n8n must run via `npx`
8. **Rate limit handling** — Use Loop, Wait, and Batch Size nodes to avoid API rate limits
9. **Language issues** — Some AI operations have trouble processing Chinese characters
10. **Never put AI Agent inside loops** — This causes errors in n8n

---

## About Me

**Chien-I (Carrie) Wen**

- M.S. in EE (CS major) — National Taiwan University (GPA 4.0/4.3, Ranked 1st)
- B.S. in Business Administration — National Cheng Kung University (GPA 3.8)
- Thesis: Monte Carlo Tree Search for Floor Layout Optimization

Bridging AI technology and business value — passionate about AI solution design, workflow automation, and helping organizations adopt AI effectively.
