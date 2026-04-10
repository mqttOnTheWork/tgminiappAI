# Sales AI — Telegram Mini App

A Telegram Mini App that gives sales professionals an AI assistant right inside Telegram. Users chat with an AI that remembers conversation context, powered by an n8n automation backend.

## Demo

The app runs as a [Telegram Mini App](https://core.telegram.org/bots/webapps) — users open it directly inside Telegram, no installation needed.

## How It Works

```
User (Telegram) → Mini App (HTML/JS) → n8n Webhook → AI Model → Response
```

1. User opens the Mini App inside Telegram
2. The frontend sends the message + session ID to an n8n webhook
3. n8n processes the request through an AI workflow with conversation memory
4. The AI response is returned and displayed in the chat interface

### Session Management

The app identifies users automatically:
- **Telegram users** → `tg_{telegram_user_id}` (extracted from Telegram WebApp API)
- **Web users** → `web_{random_uuid}` (stored in localStorage as fallback)

This ensures the AI remembers the conversation history per user.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | HTML + CSS + vanilla JS |
| Telegram integration | [Telegram WebApp JS SDK](https://core.telegram.org/bots/webapps) |
| Backend / automation | [n8n](https://n8n.io/) (webhook + AI workflow) |
| AI | Connected via n8n AI nodes (with memory) |

## Project Structure

```
├── index.html        # The entire Mini App (single-file SPA)
└── README.md
```

Yes, it's a single file — the whole app is self-contained HTML with inline CSS and JS. No build step, no dependencies, no bundler.

## Setup

### 1. n8n Workflow

Set up an n8n workflow with:
- **Webhook node** — receives `POST` requests with `{ text, sessionId }`
- **AI Agent node** — processes the message with conversation memory
- **Response node** — returns `{ answer: "..." }`

### 2. Telegram Bot

1. Create a bot via [@BotFather](https://t.me/BotFather)
2. Set the Mini App URL: `/setmenubutton` → point to your hosted `index.html`
3. Host `index.html` anywhere with HTTPS (GitHub Pages, Vercel, Netlify, etc.)

### 3. Update Webhook URL

In `index.html`, replace the webhook URL with your own:

```javascript
const WEBHOOK_URL = "https://your-n8n-instance.app.n8n.cloud/webhook/ask-ai";
```

## Features

- Real-time AI chat with conversation memory
- Automatic Telegram user identification
- Dark theme UI optimized for mobile
- Works both inside Telegram and as a standalone web page
