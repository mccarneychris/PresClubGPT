# Updated To-Do List: Build PresClubGPT in Cursor

## 🧱 Step 1: Get LibreChat Running Locally

- [x] Install Homebrew (if not already installed)
- [x] Install Node.js via Homebrew
- [x] Install Git
- [x] Install Docker (and ensure it's running)
- [x] Clone LibreChat from GitHub
- [x] Open project in Cursor
- [x] Create `.env` file from `.env.example`
- [x] Add OpenAI API Key to `.env`
- [x] Run `npm install` and `npm run build`
- [x] Run `npm run dev` and open http://localhost:3080
- [ ] Test login/signup
- [ ] Send/receive messages
- [ ] Confirm chat history
- [ ] Test file uploads (if enabled)

## 🔁 Step 2: Supabase Setup

- [ ] Create a new Supabase project
- [ ] Enable Supabase Vector extension
- [ ] Enable Supabase Auth with magic link
- [ ] Add Supabase environment variables to `.env`
- [ ] Create database tables:
  - `user_memory`
  - `chat_history`
- [ ] Create vector store table with metadata support

## 🔧 Step 3: LibreChat Customisation

- [ ] Replace auth flow with Supabase magic link (client + server)
- [ ] Add logic to include `user_id` metadata when storing/retrieving vectors
- [ ] Add persistent memory storage via `user_memory` table
- [ ] Ensure message history is stored per user

## 📦 Step 4: RAG & N8N Integration

- [ ] Build N8N workflow to:
  - Ingest sales content (frameworks, books, etc.)
  - Embed and store in Supabase Vector DB
- [ ] Enable content tagging (e.g. `is_global`, `topic`, `source`)
- [ ] Connect chat backend to query Supabase Vector DB

## 💬 Step 5: Chat UX Enhancements

- [ ] Ensure mobile responsiveness
- [ ] Display previous chat sessions
- [ ] Display inline responses for tools (placeholder logic for now)

## 🚀 Step 6: Deployment

- [ ] Create a `Dockerfile` in root of project
- [ ] Build and test Docker container locally
- [ ] Set up GCP Cloud Run deployment
- [ ] Deploy container and test full chat flow in production

## 💳 Step 7: Stripe Billing (Post-MVP)

- [ ] Create Stripe account, products, and pricing plans
- [ ] Add logic to check usage limits (messages)
- [ ] Add webhook to update user tier in Supabase
- [ ] Redirect users to Stripe Checkout for upgrade

## 🧪 Step 8: Final Testing

- [ ] Test:
  - Auth flow
  - RAG with public + private content
  - Memory persistence
  - Mobile UI
  - Vector isolation by `user_id`
- [ ] Launch closed beta with early users
