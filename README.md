# VUMI Global Health — AI Chatbot (OpenAI GPT-4o)

## Architecture

```
Browser (chatbot.html)
        |
        | HTTP POST /api/chat
        v
  Node.js Proxy Server (server/index.js)
        |
        | Bearer token auth
        v
  OpenAI API (GPT-4o)
```

## Files

| File | Purpose |
|------|---------|
| `chatbot.html` | Complete chatbot UI — deploy anywhere |
| `server/index.js` | Express proxy — keeps OpenAI key server-side |
| `server/package.json` | Node dependencies |
| `server/Dockerfile` | Docker container config |
| `server/railway.json` | One-click Railway deploy config |
| `server/.env.example` | Environment variable template |

## Quick Start

### Step 1 — Deploy the proxy server

**Option A: Railway (recommended, free tier)**
1. Push `server/` folder to a GitHub repo
2. Create new project at railway.app
3. Connect your repo
4. Add environment variable: `OPENAI_API_KEY=sk-your-key`
5. Deploy — Railway auto-detects Node.js

**Option B: Render**
1. Push `server/` to GitHub
2. New Web Service at render.com
3. Set `OPENAI_API_KEY` in environment
4. Build: `npm install` | Start: `node index.js`

**Option C: Local development**
```bash
cd server
cp .env.example .env
# Add your OPENAI_API_KEY to .env
npm install
npm start
# Server runs at http://localhost:3000
```

### Step 2 — Configure the chatbot

Open `chatbot.html` and update line 1 of the script:
```javascript
var PROXY_URL = 'https://your-app.railway.app/api/chat';
```

### Step 3 — Deploy the chatbot

The `chatbot.html` is a single self-contained file. Deploy it:
- **WhatsApp Web App**: Host on any static server (Netlify, Vercel, GitHub Pages)
- **Embed in website**: Use an `<iframe>` or copy the contents into your site
- **WhatsApp Business API**: Use the chatbot logic as the webhook response handler

## Security Notes

- The OpenAI API key lives ONLY on the server — never in the browser
- Add CORS restrictions in `server/index.js` to only allow your domain:
  ```javascript
  app.use(cors({ origin: 'https://yourdomain.com' }));
  ```
- Add rate limiting (express-rate-limit) for production use

## Customisation

- **System prompt**: Edit `SYSTEM_PROMPT` in `server/index.js` to update VUMI knowledge
- **Model**: Change `gpt-4o` to `gpt-4o-mini` for lower cost
- **Agents**: Edit the `AGENTS` array in `chatbot.html`
- **Chips**: Default chips are set after each AI response in `chatbot.html`
