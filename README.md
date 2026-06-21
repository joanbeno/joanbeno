<div align="center">

<a href="https://fixoria.com.co">
  <img src="https://raw.githubusercontent.com/joanbeno/joanbeno/main/banner.svg" alt="Nicolas Benavides — Fixoria" width="100%" />
</a>

### Business Administration · Digital Solutions Builder · Co-founder of Fixoria

I build real systems for real businesses: from AI diagnostics to industrial ERPs.<br/>
Not just strategy: code that runs in production.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-nicolasbenavides-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/nicolasbenavides)
[![Fixoria](https://img.shields.io/badge/Fixoria-fixoria.com.co-172554?style=flat-square&logo=vercel&logoColor=white)](https://fixoria.com.co)
[![Email](https://img.shields.io/badge/Email-joanbeno@unicauca.edu.co-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:joanbeno@unicauca.edu.co)

</div>

---

## Production Projects

### COTA: Industrial ERP for Machine Shops

[![Live demo](https://img.shields.io/badge/Live_demo-%E2%86%92_cota--jcb.vercel.app-172554?style=for-the-badge&logo=vercel&logoColor=white)](https://cota-jcb.vercel.app/intro)

<img src="https://raw.githubusercontent.com/joanbeno/joanbeno/main/assets/cota-preview.jpg" alt="COTA: Industrial ERP for Machine Shops" width="100%" />

A machine shop was running on Word for quotes, Excel for accounting and WhatsApp to coordinate production. COTA replaced all of that: a mobile-first web system where each action automatically feeds the next module, with no duplicate data entry. Includes Ruffo, a conversational AI agent connected to the system in real time.

**End-to-end integrated flow:**

```
Quote → client approves via public link → work order auto-generated
→ Production Kanban → Gantt by machine → payroll → DIAN invoice → collection
```

**10 modules in production:**

| Module | What it does |
|--------|-------------|
| **Quotes** | PDF with logo and letterhead, automated email delivery, client approval link. States: Draft → Sent → Approved → In Production → Invoiced → Paid |
| **Production / Work Orders** | Visual Kanban by status, job-shop Gantt organized by machine (CNC Lathe · Milling · Welding...), operator assignment and actual hours tracking |
| **Payroll** | Integrated with hours logged per work order |
| **Accounts Receivable** | Due-date traffic light with filters. Overdue · Pending · Partial · Paid |
| **Accounting** | Monthly cash flow, balance sheet, P&L, XLS export ready for the accountant |
| **Inventory** | Minimum stock with automated alerts |
| **Suppliers / POs** | Purchase orders linked to the system |
| **DIAN Invoicing** | Factus API v2 integration (Colombian electronic invoicing), numbering ranges, controlled sequence, sandbox and production |
| **Loans** | Active debt by lender, paid vs. pending installments, progress bar |
| **Budgets** | Monthly planning compared against actual execution |

**Architecture: centralized dispatcher**

All data logic flows through a single entry point: `dispatchDb()`, a `switch` with ~200 named operations (`'cotizaciones.crear'`, `'produccion.actualizarEstado'`, `'cxc.marcarPagada'`...). Server components call it directly; client components POST to `/api/sheets`, which delegates to the same dispatcher. One place where all business logic lives: auditable, extensible, no scattered endpoints.

```
Server Components  →  callSheets()      →  dispatchDb()  →  Supabase
Client Components  →  POST /api/sheets  →  dispatchDb()  →  Supabase
```

**Stack:** Next.js 16 · React 19 · TypeScript · Supabase (PostgreSQL) · NextAuth v5 · Tailwind v4 · Framer Motion · Recharts · ExcelJS · @react-pdf/renderer · Groq · Python · Telegram Bot API

---

### Ruffo: Multichannel AI Agent on COTA

> Conversational agent connected to COTA in real time. Logs expenses from photos, checks receivables, creates quotes and manages work orders from Telegram, WhatsApp or the embedded chat in the platform. LLM-agnostic architecture: works with OpenAI GPT, Google Gemini, Anthropic Claude, Groq (Llama, Qwen) or local Ollama. The provider is chosen based on the client's budget. The integration is the same in every case.

**What it does:**

- Logs expenses: send a photo of a receipt and it registers the amount, vendor and category
- Checks receivables: ask who owes what and get a live answer from the system
- Creates quotes: dictate line items from the chat and a draft is generated in COTA
- Controls work orders: query status, assign operators and update progress without opening the system
- Proactive alerts: overdue invoices, urgent receivables and weekly summaries without being asked

**Stack:** Python · Next.js · OpenAI GPT · Gemini Flash · Anthropic Claude · Groq (Llama · Qwen) · Telegram Bot API · WhatsApp Business API

---

### AI Sales Agent for WhatsApp Business

> Conversational agent in production for a garment company in Popayán, Colombia. Running 24/7 since November 2025, deployed on a private VPS with direct Meta API integration.

The client had an overwhelmed sales team, outdated inventory and was losing customers due to slow response times. The agent replaced that operational load entirely.

**What it does:**

- Sells: handles inquiries, quotes and closes orders on WhatsApp with no human involvement
- Understands voice: transcribes voice notes with Whisper and processes them like text
- Identifies products: recognizes references, garment types and variants from text, image or audio
- Size advisor: recommends size based on measurements, purchase history or customer description
- OCR: reads catalog photos, labels and physical references; extracts data to use in the conversation
- RAG: queries a business knowledge base (products, prices, policies) for accurate answers without hallucinating
- Tracks and edits inventory: checks real-time stock on Google Sheets and updates it directly from the conversation
- Alerts: proactively notifies customers: order confirmations, status updates, automatic follow-ups
- Triggers dispatch: when a sale closes, automatically emails the team via Resend to prepare and ship the order
- Pause: the team can take over manually at any point and hand control back to the agent

**Architecture:**

```
WhatsApp (customer)
  → Meta API  →  N8N (orchestrator)
                  ├── Whisper          (voice note transcription)
                  ├── OCR              (image and catalog reading)
                  ├── RAG              (business knowledge base)
                  ├── Google Sheets    (inventory and sales records)
                  ├── Resend           (dispatch email to team on sale close)
                  └── LLM              (reasoning and final response)
  → WhatsApp (reply)
```

Deployed on a private VPS. Continuously running since November 2025.

**Stack:** N8N · Meta API (WhatsApp Business) · Whisper · OCR · RAG · Google Sheets · Resend · VPS

[![Case study](https://img.shields.io/badge/Case_study-fixoria.com.co-C0392B?style=flat-square&logo=whatsapp&logoColor=white)](https://fixoria.com.co)

---

### AI Maturity Diagnostic (ML Studio)

> Strategic diagnostic tool for work teams. 12 questions, maturity profile (Explorer / Operational / Optimizer / Strategist), recommendations plan and personalized interactive guide. Integrates OpenAI GPT and Google Gemini Flash to generate recommendations adapted to each profile.

**Stack:** HTML · JavaScript · Google Apps Script · OpenAI API · Gemini Flash · Chart.js · Vercel Serverless

[![View tool](https://img.shields.io/badge/View_tool-diagnostico--fixoria.vercel.app-3535cc?style=flat-square&logo=vercel&logoColor=white)](https://diagnostico-fixoria.vercel.app)

<img src="https://raw.githubusercontent.com/joanbeno/Diagnostico-IA/main/assets/facilitador-screenshot.png" alt="Facilitator Dashboard: AI Maturity Diagnostic" width="100%" style="border-radius:8px;margin-top:8px;" />

---

### SAGEST: Organizational Learning Diagnostic

> Knowledge management assessment tool for restaurant teams. 5 dimensions · server-side scoring · gap radar · interactive guide.

**Stack:** HTML · JavaScript · Google Apps Script · Chart.js · Vercel

[![View tool](https://img.shields.io/badge/View_tool-sagest.vercel.app-1E72E4?style=flat-square&logo=vercel&logoColor=white)](https://sagest.vercel.app)
[![Technical annex](https://img.shields.io/badge/Technical_annex-architecture_and_flow-D4873A?style=flat-square)](https://sagest.vercel.app/arquitectura)

<img src="https://raw.githubusercontent.com/joanbeno/SAGEST/main/assets/preview.svg" alt="SAGEST Flow" width="100%" />

---

## Academic Projects, Universidad del Cauca

### Academic Internship Dashboard

> Real-time dashboard for tracking a doctoral AI internship in public projects. Connected to Google Sheets, shows weighted progress by phase, current week, difference vs. baseline schedule and overall status.

**Stack:** HTML · JavaScript · Google Apps Script · Google Sheets API

<img src="https://raw.githubusercontent.com/joanbeno/joanbeno/main/assets/seguimiento-preview.jpg" alt="Academic Internship Dashboard: real-time progress tracking" width="100%" style="border-radius:8px;margin-top:8px;" />

---

### Academic Tracking System (Prototype)

> Architecture proposal for an academic tracking system at Unicauca. Google OAuth + Master Sheet + Apps Script as central backend. Includes system diagram, professor hub and student guide. Cost: $0.

**Stack:** HTML · Google OAuth · Apps Script · Google Sheets

[![View prototype](https://img.shields.io/badge/View_prototype-academic_tracking_system-1E72E4?style=flat-square&logo=googlechrome&logoColor=white)](https://joanbeno.github.io/prototipo-seguimiento-/)

<img src="https://raw.githubusercontent.com/joanbeno/joanbeno/main/assets/prototipo-preview.jpg" alt="Academic Tracking System: Prototype" width="100%" style="border-radius:8px;margin-top:8px;" />

---

## Tech Stack

![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI_API-412991?style=flat-square&logo=openai&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=flat-square&logo=googlegemini&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-F55036?style=flat-square&logo=groq&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram_Bot_API-2AABEE?style=flat-square&logo=telegram&logoColor=white)
![PocketBase](https://img.shields.io/badge/PocketBase-B8DBE4?style=flat-square&logo=pocketbase&logoColor=black)
![Appwrite](https://img.shields.io/badge/Appwrite-FD366E?style=flat-square&logo=appwrite&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)
![Netlify](https://img.shields.io/badge/Netlify-00C7B7?style=flat-square&logo=netlify&logoColor=white)
![Coolify](https://img.shields.io/badge/Coolify-6C47FF?style=flat-square&logo=coolify&logoColor=white)
![Google Apps Script](https://img.shields.io/badge/Google_Apps_Script-4285F4?style=flat-square&logo=google&logoColor=white)

---

<div align="center">

**[Fixoria](https://fixoria.com.co)** · AI consulting and software development · Popayán, Colombia

</div>
