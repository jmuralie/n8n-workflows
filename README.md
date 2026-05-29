# n8n AI Workflows

A collection of production n8n automation workflows — AI research assistants, news digests, and utility bots, all triggered via Telegram.

---

## Workflows

### Smart Research Agent
**File:** `Smart_Research_Agent.json`

An AI agent (Claude claude-sonnet-4-6) that decides how to answer — directly from its knowledge, or by calling one of three tools: web search, India news, or deep page research. Supports voice notes via Whisper transcription. Maintains per-chat conversation memory.

- **Trigger:** Telegram (text or voice message)
- **Tools:** SerpAPI (Google Web), SerpAPI (Google News) + NewsData.io, Jina AI Reader
- **AI:** Claude claude-sonnet-4-6 via n8n LangChain agent
- **Memory:** Window Buffer (10 messages, keyed by chatId)
- **Credentials needed:** Telegram bot token, Anthropic API key, SerpAPI key, NewsData.io key, OpenAI key (Whisper)

---

### Voice Research Bot
**File:** `Voice_Research_Bot.json`

Accepts Telegram voice notes or text. Transcribes audio via OpenAI Whisper, runs a Google web search, and returns either a 3–5 bullet summary (normal mode) or a detailed answer from scraped pages (deep mode, triggered by keywords like "go deeper", "elaborate").

- **Trigger:** Telegram (text or voice)
- **AI:** Claude claude-sonnet-4-6 for summarization
- **Credentials needed:** Telegram bot token, OpenAI key, SerpAPI key, Anthropic API key

---

### Research Assistant
**File:** `Research_Assistant.json`

Routes queries to general web search (SerpAPI + Brave Search) or India news (SerpAPI Google News + NewsData.io) based on message prefix (`india:` prefix → India route). Summarizes results into 3–5 bullet points.

- **Trigger:** Telegram
- **AI:** Claude claude-sonnet-4-6
- **Routing:** `india:` prefix → India news path; plain text → general web

---

### Daily Air Quality Bot
**File:** `Daily_Air_Quality_Report.json`

On-demand Telegram bot that fetches real-time air quality data (Open-Meteo) and pollen levels (Ambee API — tree, grass, weed) and sends a formatted report to the requesting chat.

- **Trigger:** Telegram message (any text)
- **Data sources:** Open-Meteo API (AQI, PM2.5, ozone), Ambee Pollen API

---

### TN News Digest
**File:** `TN_news.json`

Fetches the top 10 Tamil Nadu news articles and sends them as a Telegram media album — each article as an image with title, source, and link. Falls back to a text list if images are unavailable.

- **Trigger:** Scheduled
- **Output:** Telegram `sendMediaGroup` album

---

### Slack to Asana Task
**File:** `Slack_to_asana_task.json`

Listens for a Slack trigger and creates a task in Asana with the message content.

- **Trigger:** Slack
- **Credentials needed:** Slack token, Asana token

---

### Vendor Status Checker
**File:** `Vendor_Status_Checker.json`

Checks vendor status (workflow-specific logic) and reports results.

---

## Setup

1. Import the desired `.json` file into your n8n instance via **Settings → Import Workflow**
2. Fill in credentials (Telegram bot token, API keys) in the n8n Credentials panel
3. Activate the workflow

### Tested environment

- n8n running in Docker, port 5678
- Telegram webhooks via Tailscale Funnel (HTTPS)
- Anthropic credential: n8n built-in `lmChatAnthropic` node (typeVersion 1.1)

> **Note on Anthropic credentials:** Use `lmChatAnthropic` typeVersion `1.1` (not 1.3). Version 1.3 expects a structured `model.value` object; 1.1 uses a plain model ID string and works with standard n8n credential import.

---

## License

MIT
