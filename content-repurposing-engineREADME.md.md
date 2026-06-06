# Content Repurposing Engine

An n8n automation agent that ingests long-form articles, processes them through a specialized LLM prompt, and auto-dispatches three platform-optimized marketing assets — a LinkedIn post, a Twitter/X thread, and an email newsletter teaser — directly to a Gmail inbox for review.

## System Architecture

```
Manual Trigger
      │
      ▼
JavaScript Sanitizer        ← Strips HTML tags, normalizes whitespace
      │
      ▼
Groq LLM Chain              ← Llama 3.3 70b-versatile via Groq API
(+ System Prompt)           ← Structured prompt with exact output delimiters
      │
      ▼
JavaScript Parser           ← Splits raw LLM output into 3 labeled assets
      │
      ▼
Gmail Dispatcher            ← Sends formatted HTML email to reviewer inbox
```

## Tech Stack

| Component | Tool |
|-----------|------|
| Orchestration | n8n (Community / Self-Hosted) |
| Runtime Logic | JavaScript (ES6) |
| AI Model | Groq Cloud API — `llama-3.3-70b-versatile` |
| Email Dispatch | Gmail OAuth2 via n8n |

## Output Assets

| Asset | Format | Constraints |
|-------|--------|-------------|
| LinkedIn Post | Long-form professional copy | Authority hook, no emojis, structural line breaks |
| Twitter/X Thread | 3-part micro-thread | Single core argument, explicit CTAs per tweet |
| Email Teaser | Conversational intro paragraph | High-open-rate curiosity hook |

## Quick Start

### 1. Install n8n locally

```bash
npm install -g n8n
n8n start
```

Open your browser at `http://localhost:5678`

### 2. Import the workflow

- Open n8n → click **+** to create a new workflow
- Press `Ctrl + V` (or `Cmd + V`) directly on the canvas
- Paste the full contents of `workflow.json`

### 3. Configure credentials

In n8n under **Settings → Credentials**, add:

| Credential Name | Type | Where to get it |
|----------------|------|-----------------|
| `Groq account` | Groq API | https://console.groq.com/keys |
| `Gmail account` | Gmail OAuth2 | Google Cloud Console → OAuth2 |

> **Never paste API keys into the workflow JSON directly.** Always use n8n's credential manager.

### 4. Set your reviewer email

In the **Send a message** (Gmail) node, update the `sendTo` field to your target inbox.

### 5. Add your article and run

In the **When clicking 'Execute workflow'** trigger node, paste your article text into the `article_text` field under **Pin Data**, then click **Execute workflow**.

## Input / Output

**Input:** Raw article text (plain text or HTML — the sanitizer strips tags automatically)

**Output:** A single HTML-formatted email delivered to the configured inbox containing all three assets, labelled and separated.

## Modifying the Prompt

The LLM instruction logic is documented in `system_prompt.txt`. To update it:

1. Edit `system_prompt.txt`
2. Copy the updated prompt text into the **Basic LLM Chain** node → **Messages** field in n8n
3. Commit both files together so the repo stays in sync with the live workflow

## Environment Variables (if running n8n via Docker or CLI)

```bash
GROQ_API_KEY=your_key_here        # Never commit this value
GMAIL_CLIENT_ID=your_client_id
GMAIL_CLIENT_SECRET=your_secret
```

These are passed to n8n at runtime and are never stored in this repository.
