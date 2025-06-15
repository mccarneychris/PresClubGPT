# ✅ To-Do List: Build PresClubGPT in Cursor

## 🧱 Project Setup

- [ ] Fork LibreChat from GitHub
- [ ] Create a new local project in Cursor using the fork
- [ ] Rename remote `origin` to `upstream` for tracking updates
- [ ] Create a new branch: `presclubgpt-dev`
- [ ] Install project dependencies
- [ ] Set up `.env` file with placeholders:
  - `SUPABASE_URL`
  - `SUPABASE_ANON_KEY`
  - `OPENAI_API_KEY`
  - `STRIPE_SECRET_KEY`

---

## 🔐 Supabase Setup

- [ ] Create a new Supabase project
- [ ] Enable Supabase Vector extension
- [ ] Enable Supabase Auth with magic link
- [ ] Add Supabase environment variables to `.env`
- [ ] Create database tables:
  - `user_memory`
  - `chat_history`
- [ ] Create vector store table with metadata support

---

## 🔧 LibreChat Customisation

- [ ] Replace auth flow with Supabase magic link (client + server)
- [ ] Add logic to include `user_id` metadata when storing/retrieving vectors
- [ ] Add persistent memory storage via `user_memory` table
- [ ] Ensure message history is stored per user

---

## 📦 RAG & N8N Integration

- [ ] Build N8N workflow to:
  - Ingest sales content (frameworks, books, etc.)
  - Embed and store in Supabase Vector DB
- [ ] Enable content tagging (e.g. `is_global`, `topic`, `source`)
- [ ] Connect chat backend to query Supabase Vector DB

---

## 💬 Chat UX Enhancements

- [ ] Ensure mobile responsiveness
- [ ] Display previous chat sessions
- [ ] Display inline responses for tools (placeholder logic for now)

---

## 🚀 Deployment

- [ ] Create a `Dockerfile` in root of project
- [ ] Build and test Docker container locally
- [ ] Set up GCP Cloud Run deployment
- [ ] Deploy container and test full chat flow in production

---

## 💳 Stripe Billing (Post-MVP)

- [ ] Create Stripe account, products, and pricing plans
- [ ] Add logic to check usage limits (messages)
- [ ] Add webhook to update user tier in Supabase
- [ ] Redirect users to Stripe Checkout for upgrade

---

## 🧪 Final Testing

- [ ] Test:
  - Auth flow
  - RAG with public + private content
  - Memory persistence
  - Mobile UI
  - Vector isolation by `user_id`
- [ ] Launch closed beta with early users

---

## 📌 Stretch Goals (Future)

- [ ] Workspace/org sharing model
- [ ] Manual tool triggers (forms/buttons)
- [ ] Admin dashboard
- [ ] Custom memory service (Zep or similar)
---

## ⚠️ Edge Case Tasks

- [ ] Add "resend magic link" support and expired link messaging
- [ ] Validate Supabase session expiration and reuse handling
- [ ] Ensure user email can be reused across workspaces
- [ ] Add upload validation logic to allow only admin to set global tagging
- [ ] Implement file upload rate-limiting or batching
- [ ] Confirm separation of memory vs RAG in prompt logic
- [ ] Add fallback messaging for failed tool executions
- [ ] Implement upgrade CTA for users who hit message quota
- [ ] Add logging or retry mechanism for Stripe webhooks
- [ ] Add onboarding tips for first-time users (e.g., "Try MEDDIC...")
- [ ] Make assistant explain when it auto-triggers a tool
