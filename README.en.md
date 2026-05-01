# 🤖 WhatsApp & Instagram AI Bot Framework

> Multichannel conversational agent built with Node.js + OpenAI + Meta API.  
> Multi-client architecture: one base backend adapted to each industry with its own flows, pricing and business logic.

---

## 🚀 Live demo

🎥 **Video demo:** [Watch on YouTube](https://www.youtube.com/watch?v=_C-984BwlBQ)  
🌐 **Frontend:** [web-page-saa-s.vercel.app](https://web-page-saa-s.vercel.app)  
💬 **Chatbot widget:** available in the bottom-right corner of the site

---

## 🏗️ Base architecture

```
┌──────────────────┐     ┌─────────────────────┐     ┌──────────────────┐
│  Instagram DMs   │────▶│                     │────▶│  OpenAI GPT-4o   │
│  WhatsApp        │     │   Node.js + Express │     │  (system prompt  │
│  Web Chat        │────▶│      Backend        │     │   per client)    │
└──────────────────┘     │                     │     └──────────────────┘
                         │  • Webhook handler  │
                         │  • State machine    │────▶┌──────────────────┐
                         │  • Deduplication    │     │    Supabase      │
                         │  • Session + lang   │     │  (PostgreSQL)    │
                         │  • Human takeover   │     └──────────────────┘
                         │  • Email notif.     │
                         └─────────────────────┘
                                    │
                         ┌──────────┴──────────┐
                         │   HTML Dashboard    │
                         │   (per client)      │
                         └─────────────────────┘
```

---

## 📦 Deployed projects

### 🎞️ Film Lab — Instagram Bot (Mexico City)
**Repo:** [client-foto](https://github.com/ayrtoncela/client-foto)

Analog photography lab in Mexico City. Instagram bot that handles the full order flow for film developing and scanning.

| Channel | Stack | Status |
|---|---|---|
| Instagram DMs | Node.js + Supabase + Railway + Resend | ✅ Production |

**Features:**
- Full state machine: quote → order → payment → delivery
- Automatic language detection (ES/EN)
- 3 drop-off locations + home pickup by neighborhood in CDMX
- Order number generation (ORD-YYYYMM-XXXX) + email notification
- Payment receipt via image DM
- Operations dashboard with visual pipeline, payment validation and human handoff
- Support for 35mm and 120 formats with differentiated pricing

---

### 🎨 Mikaela Montenegro — Instagram Bot + Workshop system

Ecuadorian visual artist with exhibitions in NYC, Paris and Quito. Instagram bot focused on boosted reel campaigns, with art workshop management.

| Channel | Stack | Status |
|---|---|---|
| Instagram DMs | Node.js + Supabase + Railway | ✅ Production |

**Features:**
- Bot only responds to DMs originating from boosted reels (matched by Meta `post_id`)
- Each campaign has its own context: workshop, price, schedule, location, capacity
- Numeric max capacity: auto-pauses the campaign when the last student enrolls
- Already-enrolled students keep receiving responses even when campaign is paused
- Per-conversation killswitch (take over / release bot)
- AI Draft for manual copy-paste when the bot can't respond (24h window constraint)
- Dashboard: enroll students from conversation, workshop badges, real-time counter
- Bilingual website (ES/EN) with exhibition pages, artwork slideshow and purchase modal

---

### 🧪 Clinical Laboratory — WhatsApp Bot
**Channel:** WhatsApp + Web Chat

Structured 5-step appointment booking bot for clinical studies.

| Channel | Stack | Status |
|---|---|---|
| WhatsApp + Web | Node.js + Google Sheets + Railway | ✅ Live demo |

**Features:**
- Step-by-step booking flow (study type → branch → day → time → contact info)
- 3 branches with embedded schedules
- Pre-analysis instructions by study type
- Automatic Google Sheets logging + email notification per lead
- Message deduplication (in-memory cache)

---

### 💻 Software Agency — Web Chat Bot
**Channel:** Web

Lead intake bot for a software development agency.

**Features:**
- Service and use-case presentation
- Contact capture and lead qualification
- Same backend as WhatsApp, web channel via `/api/chat`

---

### 🍽️ Restaurant — Web Chat Bot
**Channel:** Web

Reservations and diner support bot.

**Features:**
- Menu and hours inquiry
- Reservation flow
- Order handling

---

## ✨ Framework features

**Conversation**
- Per-client state machine (structured flows that stay on track)
- Session memory with configurable timeout
- Automatic language detection (ES/EN)
- Global keywords for menu reset (`menu`, `start`, `inicio`)
- OpenAI fallback for out-of-flow messages

**Operations**
- Per-client HTML dashboard with authentication
- Human takeover — pauses the bot and enables manual agent response
- Per-conversation killswitch
- Visual order status pipeline
- Push notifications to client via Instagram/WhatsApp
- Incoming image support (payment receipts, photos)

**Infrastructure**
- Message deduplication via Supabase (`processed_messages`)
- Configurable session timeout
- Transactional email via Resend
- Railway deploy with environment variables

---

## 🛠️ Full stack

| Layer | Technology |
|---|---|
| Backend | Node.js 18 + Express |
| AI | OpenAI GPT-4o-mini |
| Messaging | Meta Graph API (Instagram + WhatsApp) |
| Database | Supabase (PostgreSQL) |
| Email | Resend |
| Deploy | Railway |
| Dashboard frontend | Vanilla HTML/CSS/JS |
| Web frontend | Vercel |

---

## 👨‍💻 Author

**Ayrton Cela** — Consulting Engineering Manager & AI Builder  
Mexico City 🇲🇽

**WhatsApp:** [+52 5544621764](https://wa.me/525544621764)  
**Email:** ayrtoncela94@gmail.com  
**GitHub:** [github.com/ayrtoncela](https://github.com/ayrtoncela)

> *Built with help from Claude*
