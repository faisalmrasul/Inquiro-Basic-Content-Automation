# 🚀 Autonomous AI Content Repurposing Engine
<img width="1192" height="363" alt="image" src="https://github.com/user-attachments/assets/4bf80e5a-80ca-4970-9a67-e03bd3cfabc6" />

A production-grade, asynchronous automation pipeline that ingests long-form articles, processes them through specialized LLM constraints, and outputs platform-optimized marketing assets directly to an execution queue.

## 🏗️ System Architecture & Data Flow
The engine is built entirely on an open-source framework, eliminating dependency on premium automation tiers. 

1. **Trigger Phase:** Manual workflow invocation triggers synchronous runtime payload ingestion.
2. **Sanitization Phase:** Custom JavaScript parsing cleans the data and structures it into optimized JSON strings.
3. **Reasoning Phase:** Data is dynamically routed to the **Gemma2-9b-it** model via the **Groq API** (architected intentionally to maximize Token-Per-Minute quotas on free-tier infrastructure).
4. **Parsing Phase:** The raw LLM string token is broken down by exact block delimiters (`email_teaser`, `linkedin_post`, `twitter_thread`).
5. **Dispatch Phase:** The parsed content is compiled into HTML bodies and automatically sent via the **Gmail API** directly to the manager's review desk.

## 🛠️ Tech Stack
* **Orchestration:** n8n (Community/Self-Hosted Edition)
* **Runtime Logic:** JavaScript (ES6)
* **AI Model Engine:** Groq Cloud API (`gemma2-9b-it`)
* **Integrations:** Google Workspace (Gmail SMTP OAuth2)

## 📂 How to Deploy This Local Instance
To clone this workflow and run it permanently on your machine for free:
1. Download and install [Node.js](https://nodejs.org/).
2. Initialize n8n locally via your command terminal: `npm install -g n8n` followed by `n8n start`.
3. Create a blank workflow in your local n8n panel.
4. Copy the entire raw text content from the `workflow.json` file in this repository.
5. Paste (`Ctrl + V`) directly onto your n8n canvas map.
6. Re-link your private Groq and Gmail credentials.

---
*Developed by Faisal Moarafur Rasul — Built for scalable, zero-overhead marketing automation.*
