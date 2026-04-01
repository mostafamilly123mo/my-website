---
title: 'OpenClaw: Turning My MacBook into a Personal AI Assistant'
description: 'How I used OpenClaw with Qwen3.5 via Ollama to supercharge Siri with real AI capabilities—scheduling cron jobs, crawling tech news, managing meetings, and more—all running locally at zero API cost.'
heroImage: 'https://github.com/user-attachments/assets/76567b4d-199c-4cec-8bb5-01e403e2fb79'
categories: ['AI', 'Productivity']
tags: ['AI', 'OpenClaw', 'Ollama', 'Qwen', 'Automation', 'MacOS', 'Local AI']
pubDate: '2026-04-01T15:00:00.000Z'
---

### OpenClaw Is Changing the Way I Use My MacBook

I have been experimenting with local AI tooling for a while, but nothing shifted my workflow quite like [OpenClaw](https://github.com/openclaw). It lets you run an AI agent gateway on your own machine—no cloud API bills, no data leaving your device—and connect it to almost anything you already use.

The short version: I created a custom Siri shortcut on my Mac that talks to the OpenClaw Gateway via a simple HTTP call. Now when I say "Hey Siri, summarize this week's tech news," I actually get a useful answer instead of a web search result 😂.

---

### What Is OpenClaw?

OpenClaw is a locally hosted AI agent framework. It exposes a **gateway** that your apps (or Siri shortcuts, cron jobs, scripts, etc.) can call, and it routes those requests to a local LLM. In my setup I am using **Qwen3.5** served by **Ollama**, which means:

- **No API costs** — the model runs entirely on my machine.
- **No data leakage** — nothing is sent to a third-party provider.
- **Low latency** — for most tasks the response is faster than a cloud round-trip.

---

### Giving Siri Actual AI Powers

Setting up the Siri shortcut took about ten minutes:

1. Open the **Shortcuts** app on macOS.
2. Create a new shortcut that uses the **"Get Contents of URL"** action pointed at `http://localhost:<port>/api/chat`.
3. Pass the spoken input as the request body in JSON.
4. Show the response in a notification or read it aloud with **"Speak Text"**.

Now Siri is essentially a voice front-end for a real LLM. I can ask it to draft a tweet, summarise a document, or kick off one of my OpenClaw automations—all hands-free.

---

### Cool Things You Can Do with OpenClaw

#### Crawl & Summarize Tech News Weekly

Set up a cron job that asks OpenClaw to fetch the latest headlines, summarize them, and push the digest straight to a **Telegram** channel:

```bash
# crontab -e
0 9 * * 1  curl -s -X POST http://localhost:8080/api/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "Crawl the top tech news from this week and send a summary to the Telegram news channel."}'
```

Every Monday morning my Telegram has a clean briefing waiting for me—no manual effort required.

#### Manage Your Calendar and Meetings

OpenClaw can be given tools to read and write calendar events. Ask it to:

- "Schedule a 30-minute sync with the team next Tuesday at 10 AM."
- "List all my meetings tomorrow and draft a prep agenda."

#### Automate Social Media Posts

Draft and schedule posts across platforms by wiring OpenClaw to the relevant APIs. You describe what you want to say; the agent handles tone, formatting, and timing.

---

### A Word on Security

Giving an AI agent shell-level access to your machine is powerful—and comes with real risks. A few things I do to keep things safe:

- **Run OpenClaw inside Docker.** Containerizing the gateway limits what it can touch on the host system.
- **Restrict network access.** Use Docker network policies so the container cannot reach anything it does not need.
- **Audit the tools you enable.** Only activate the integrations (filesystem, calendar, Telegram, etc.) you actually use.
- **Never expose the gateway port publicly.** Keep it bound to `localhost` unless you have a proper auth layer in front of it.

Defense in depth is key when you are automating things that matter.

---

### Running Qwen3.5 Locally with Ollama

If you have not tried Ollama yet, getting a model running is remarkably straightforward:

```bash
# Install Ollama (macOS)
brew install ollama

# Pull Qwen3.5
ollama pull qwen3.5

# Start the server
ollama serve
```

Point OpenClaw at `http://localhost:11434` as its LLM backend and you are good to go.

---

### Conclusion

Local AI tooling has crossed a threshold where it is genuinely useful for everyday tasks, not just demos. OpenClaw makes it easy to compose those capabilities into real automations—and running everything on-device means you keep full control over your data and your costs.

Are you experimenting with AI agent tooling, or are you still cautious about letting AI automate your machine? I would love to hear what you are building.

**#OpenClaw #Qwen3_5 #LocalAI #Ollama #MacOS**
