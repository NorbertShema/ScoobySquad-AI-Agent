# ScoobySquad AI Agent

> An AI-powered customer service and booking agent built for a real, live pet waste removal business — handling 100% of customer inquiries and service bookings autonomously.

🔗 [scoobysquad.com](https://scoobysquad.com/)

---

## What is ScoobySquad?

ScoobySquad is a local pet waste removal service — think lawn care, but just for cleaning up after dogs. The business was growing fast, but the owner was spending hours every week manually responding to the same customer questions over DMs, emails, and contact forms: pricing, availability, service areas, and how to book.

My friend Michael and I built an AI agent to solve that. Customers now visit the site, ask questions, get instant, accurate answers, and can book a service — all without anyone touching it.

**Live users: 20 | Human interventions required: 0**

---

## Demo

> 📸 *Screenshots/screen recording coming soon*

<!-- Add demo GIF or screenshots here -->

---

## Features

- **Conversational AI** — answers customer questions naturally using real business data
- **Autonomous booking** — creates actual service bookings end-to-end without human input
- **Authenticated sessions** — secure user identity via Google OAuth (Supabase Auth)
- **Conversation logging** — all sessions logged to Google Sheets for owner visibility
- **Live CRM integration** — agent pulls real customer and availability data via Sweep&Go API
- **SEO-optimized frontend** — built with Next.js SSR for local search discoverability

---

## Architecture

```
User (Browser)
     │
     ▼
┌─────────────────────────┐
│   Next.js Frontend      │  ← SSR for SEO, Material UI, Google OAuth
│   (Vercel)              │
└────────────┬────────────┘
             │ Authenticated webhook (Supabase session token)
             ▼
┌─────────────────────────┐
│   n8n Workflow Engine   │  ← Orchestrates logic, escalation routing,
│                         │    tool calls, and API integrations
└──┬──────────┬───────────┘
   │          │
   ▼          ▼
┌──────┐  ┌───────────────────┐
│Gemini│  │  Sweep&Go API     │  ← Live CRM: customer data, availability,
│ LLM  │  │  (Business CRM)   │    service area info fed to agent
└──────┘  └───────────────────┘
   │
   ▼
┌─────────────────────────┐
│   Supabase (PostgreSQL) │  ← FAQ storage, session data,
│                         │    structured querying + built-in auth
└─────────────────────────┘
   │
   ▼
┌─────────────────────────┐
│   Google Sheets         │  ← Conversation logs via Google Cloud API
│   (Owner Dashboard)     │    — zero learning curve for the owner
└─────────────────────────┘
```

---

## Backend Overview

The backend is built around **n8n** as the central workflow engine rather than a custom Express/FastAPI server. This was a deliberate tradeoff: n8n gave us complex logic handling (escalation routing, conditional branching, multi-step tool calls) without reinventing the wheel — critical given a limited budget.

**Key decisions:**

- **n8n over custom backend** — handles workflow orchestration and API chaining out of the box. Saved weeks of backend development time.
- **Supabase over Firebase** — PostgreSQL gave us cleaner structured querying for FAQs and session data. The built-in auth handled Google OAuth without any extra setup.
- **Gemini as the LLM** — free tier, capable, and easy to integrate into n8n workflows.
- **Sweep&Go API for CRM** — the agent is grounded in real, live business data at all times. No hallucinated pricing or availability.
- **Google Sheets for logging** — not the most sophisticated data store, but it's what the owner already used. Reducing adoption friction mattered more than technical elegance here.

**Authentication flow:**
Frontend authenticates users via Supabase Google OAuth → session token is passed with every webhook call to n8n → n8n validates the token against Supabase before processing any request.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js, Material UI |
| Workflow Engine | n8n |
| LLM | Google Gemini |
| Database | Supabase (PostgreSQL) |
| Auth | Supabase Auth (Google OAuth) |
| CRM | Sweep&Go API |
| Logging | Google Sheets (Google Cloud API) |
| Hosting | Hostinger /Vercel |

---

## Lessons Learned

**Ground your AI in real data.** Early versions of the agent gave confident but wrong answers about pricing and availability because it was working from static text. Integrating the live CRM via Sweep&Go fixed this entirely — accurate responses require accurate data, not just a smart model.

**Match your tools to your user, not your preferences.** I would have used a proper database for conversation logging, but the owner was already comfortable with Google Sheets. The best solution is the one people actually use.

**$0 budget forces better decisions.** Every architecture choice had to be justified. That constraint made me think harder about tradeoffs — and the result is a system that's simpler, faster to maintain, and easier to hand off.

---

## Built By

- **Norbert & Micheal** — architecture, development, CRM integration, testing
- **Michael & Norbert** — development, product direction, business context, project management

---

*Have questions or want to talk through the architecture? Reach out at shemanorbert11@gmail.com*
