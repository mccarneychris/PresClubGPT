# Product Requirements Document (PRD)

## Project: PresClubGPT (Tech Sales Coaching SaaS)

### Author: Chris  
### Last Updated: 2025-06-15

---

## 📌 Overview

Build a SaaS coaching assistant for tech salespeople using a modified version of LibreChat. The system will provide AI-powered coaching based on industry-standard frameworks and personal inputs. Users will interact with a chat UI enriched by a vector database and persistent memory, and will later access custom productivity tools.

---

## 🧩 Core Goals

- Offer tailored coaching via chat
- Use RAG (Retrieval-Augmented Generation) from a vector DB
- Isolate user-specific documents and memory
- Launch with a free tier and support paid plans
- Deploy as a fully hosted SaaS with Stripe integration

---

## 🛠️ Tech Stack

| Component        | Tool / Service         |
|------------------|------------------------|
| Chat UI          | Fork of LibreChat      |
| Backend API      | LibreChat (Node.js)    |
| Auth             | Supabase (Magic Link)  |
| Database         | Supabase Postgres      |
| Vector Storage   | Supabase Vector        |
| Hosting          | GCP Cloud Run (Docker) |
| Payments         | Stripe                 |
| Workflow Engine  | N8N                    |
| Dev Environment  | Cursor (Mac)           |

---

## 📦 Feature Set (MVP)

### ✅ Must Have

- User registration & login (magic link via Supabase Auth)
- Chat interface powered by LLM API (OpenAI, Anthropic, etc.)
- Vector search across:
  - Shared public knowledge (e.g. MEDDIC, Force Mgmt)
  - User-specific uploads (isolated by `user_id`)
- Basic persistent memory (stored in `user_memory` table)
- Message history and chat sessions
- Mobile-friendly design

### 🧪 Nice to Have (Later)

- Stripe billing (free tier, usage-limited, paid tier)
- Workspace/Org model (shared memory in enterprise plans)
- Account research, doc generation tools (triggered inline)
- Admin dashboard & usage analytics

---

## 🧱 Supabase Schema (Initial)

```sql
-- Users (managed by Supabase Auth)
-- No manual table creation needed

-- User Memory
CREATE TABLE user_memory (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id uuid REFERENCES auth.users ON DELETE CASCADE,
  memory_type TEXT,  -- e.g., "preference", "session_summary"
  content TEXT,
  created_at TIMESTAMP DEFAULT now()
);

-- Chat History
CREATE TABLE chat_history (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id uuid REFERENCES auth.users ON DELETE CASCADE,
  session_id TEXT,
  message TEXT,
  role TEXT, -- "user" or "assistant"
  created_at TIMESTAMP DEFAULT now()
);

-- Vector DB Schema (supabase vector plugin)
-- Each record must have: id, content, embedding, metadata
-- Metadata to include user_id and is_global flag
```

---

## 🔄 Application Flow

### 1. User Flow

```plaintext
User registers (magic link) → lands on chat → enters question
→ RAG searches both global and user-specific vectors
→ LLM generates coaching response
→ Store chat in history + memory table
```

### 2. Dev & Deployment Flow

```plaintext
LibreChat Fork → Add Supabase Auth → Embed RAG logic
→ Use Docker to containerise → Deploy to GCP Cloud Run
→ Stripe integrated post-MVP
```

---

## 🚢 Deployment Targets

- Supabase (project, vector extension, tables, storage)
- GCP Cloud Run (container built via Docker)
- .env setup with:
  - `SUPABASE_URL`
  - `SUPABASE_ANON_KEY`
  - `OPENAI_API_KEY`
  - `STRIPE_SECRET_KEY` (optional in MVP)

---

## 🚀 Post-MVP Roadmap

| Feature                  | Priority |
|--------------------------|----------|
| Stripe billing           | High     |
| Inline tools (functions) | High     |
| Workspace/org support    | Medium   |
| Admin analytics panel    | Low      |
| Persistent chat memory   | Medium   |

---

## ✅ Launch Checklist

- [ ] Fork & modify LibreChat (auth, vector hooks)
- [ ] Supabase setup (auth, tables, RAG-ready vector index)
- [ ] Cursor local dev config working
- [ ] Dockerfile created & tested locally
- [ ] Deploy to GCP Cloud Run
- [ ] Test chat flow with public and user-specific data
- [ ] Add persistent memory table logic
- [ ] Finalise branding + onboarding flow
- [ ] Define trial usage rules
- [ ] Enable Stripe billing + webhooks

---

## ✍️ Notes

- Custom tool execution will start automatically but must be designed for manual triggers later.
- LLM provider should be configurable via `.env` for flexibility.
- Data isolation is enforced through metadata filtering in Supabase Vector queries.
---

## ⚠️ Considered Edge Cases

### 🔐 Authentication & Access
- Include a "resend magic link" flow and clear messaging for expired links
- Ensure sessions are short-lived and properly validated to prevent reuse
- Support users using the same email across different workspaces

### 📦 Vector Data Isolation & Retrieval
- Only admins can upload global content — tagging logic must be validated during ingestion
- Rate-limit file uploads and throttle large batches to avoid API overload and cost spikes

### 💬 Chat Logic / Assistant Behaviour
- Ensure prompts clearly separate long-term memory vs retrieved RAG context (unless LibreChat already handles this)
- Add fallback response when tool-triggered actions fail:  
  “I attempted to generate a doc, but something went wrong. Try again or refresh.”

### 📈 Billing & Usage Limits
- Gracefully handle quota exhaustion mid-session with an upgrade CTA
- Add Stripe webhook retry or logging to avoid broken subscription states

### 🌍 User Experience
- Add onboarding cues like: “Try asking how to qualify a lead using MEDDIC.”
- Make sure the assistant clearly states when it's auto-triggering a tool
