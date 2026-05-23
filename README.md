<div align="center">

<br/>

<img src="https://img.shields.io/badge/StudyAI-AI%20Learning%20OS-7c6ff7?style=for-the-badge&logo=sparkles&logoColor=white" height="36"/>

<br/><br/>

<p>
  <img src="https://img.shields.io/badge/Next.js-15-000000?style=flat-square&logo=nextdotjs&logoColor=white"/>
  <img src="https://img.shields.io/badge/FastAPI-0.111-009688?style=flat-square&logo=fastapi&logoColor=white"/>
  <img src="https://img.shields.io/badge/TypeScript-5.4-3178c6?style=flat-square&logo=typescript&logoColor=white"/>
  <img src="https://img.shields.io/badge/Tailwind_CSS-3.4-38bdf8?style=flat-square&logo=tailwindcss&logoColor=white"/>
  <img src="https://img.shields.io/badge/OpenAI-GPT--4o-412991?style=flat-square&logo=openai&logoColor=white"/>
  <img src="https://img.shields.io/badge/LangChain-RAG-1c3c3c?style=flat-square&logo=chainlink&logoColor=white"/>
  <img src="https://img.shields.io/badge/PostgreSQL-16-4169e1?style=flat-square&logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/ChromaDB-Vector_DB-F46800?style=flat-square"/>
  <img src="https://img.shields.io/badge/Docker-Ready-2496ED?style=flat-square&logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
</p>

<h3>Learn smarter, not harder.</h3>

<p>StudyAI is a <strong>production-grade, full-stack AI study companion</strong> that lets students upload notes, chat with their material using RAG, generate adaptive quizzes, plan personalized study schedules, and track progress — all powered by GPT-4o and LangChain.</p>

<br/>

[Live Demo](https://techhgirlie.github.io/studyai/) · [Report a Bug](../../issues) · [Request a Feature](../../issues)

<br/>

---

</div>

## 📋 Table of Contents

- [✨ Features](#-features)
- [🖼 Screenshots](#-screenshots)
- [🏗 Architecture](#-architecture)
- [📁 Project Structure](#-project-structure)
- [🗄 Database Schema](#-database-schema)
- [🤖 RAG Pipeline](#-rag-pipeline)
- [🚀 Getting Started](#-getting-started)
- [⚙️ Environment Variables](#-environment-variables)
- [🔌 API Reference](#-api-reference)
- [🐳 Docker Deployment](#-docker-deployment)
- [🚢 Production Deployment](#-production-deployment)
- [🛠 Tech Stack](#-tech-stack)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

---

## ✨ Features

| Feature | Description | Status |
|---|---|---|
| 🔐 **Authentication** | NextAuth v5 — Google, GitHub & email/password | ✅ |
| 📊 **Dashboard** | Study streak, hours, quiz scores, AI insights, upcoming exams | ✅ |
| 📄 **Upload Notes** | Drag-and-drop PDF, DOCX, TXT, MD with background AI processing | ✅ |
| 💬 **AI Chat (RAG)** | Streaming chat against your documents with source citations | ✅ |
| ⚡ **Quiz Generator** | MCQ, True/False, Short Answer — AI-generated with difficulty levels | ✅ |
| 🗂 **Flashcards** | SM-2 spaced-repetition flashcard system generated from notes | ✅ |
| 📅 **Study Planner** | AI-generated timetables aware of exam dates and priorities | ✅ |
| ⏱ **Focus Mode** | Pomodoro timer with session tracking and ambient statistics | ✅ |
| 📈 **Progress Analytics** | Study hours, quiz scores, subject breakdown, weak area detection | ✅ |
| 🌙 **Dark / Light Mode** | Full theme support with system preference detection | ✅ |

---

## 🖼 Screenshots

> _Dashboard, AI Chat, Quiz Engine, Focus Mode, and Progress Analytics_

| Dashboard | AI Chat |
|---|---|
| ![Dashboard](docs/screenshots/dashboard.png) | ![Chat](docs/screenshots/chat.png) |

| Quiz Engine | Focus Mode |
|---|---|
| ![Quiz](docs/screenshots/quiz.png) | ![Focus](docs/screenshots/focus.png) |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT                               │
│              Next.js 15  ·  Tailwind  ·  Framer Motion      │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTPS
         ┌───────────────┴────────────────┐
         │                                │
         ▼                                ▼
┌─────────────────┐             ┌──────────────────┐
│  Next.js API    │             │   FastAPI Python  │
│  Routes (Edge)  │             │   (AI Services)   │
│                 │             │                   │
│  /api/chat      │             │  /api/documents   │
│  /api/documents │             │  /api/quiz        │
│  /api/quiz      │             │  /api/analytics   │
│  /api/planner   │             │  /api/search      │
│  /api/focus     │             └────────┬─────────┘
└────────┬────────┘                      │
         │                               │
         ▼                               ▼
┌─────────────────┐             ┌──────────────────┐
│   PostgreSQL    │             │    ChromaDB       │
│   (Prisma ORM)  │             │  (Vector Store)   │
│                 │             │                   │
│  Users          │             │  Document chunks  │
│  Documents      │             │  Embeddings       │
│  Chats          │             │  Semantic index   │
│  Quizzes        │             └──────────────────┘
│  Study Plans    │
│  Focus Sessions │             ┌──────────────────┐
└─────────────────┘             │     OpenAI        │
                                │  GPT-4o (chat)    │
                                │  text-embedding   │
                                │  -3-small (RAG)   │
                                └──────────────────┘
```

---

## 📁 Project Structure

```
studyai/                          # Turborepo monorepo root
│
├── apps/
│   ├── web/                      # Next.js 15 frontend
│   │   ├── src/
│   │   │   ├── app/
│   │   │   │   ├── api/          # API routes
│   │   │   │   │   ├── chat/           route.ts   ← Streaming RAG chat
│   │   │   │   │   ├── documents/      route.ts   ← Upload & list docs
│   │   │   │   │   ├── quiz/generate/  route.ts   ← AI quiz generation
│   │   │   │   │   ├── planner/generate/route.ts  ← AI study plan
│   │   │   │   │   └── focus/          start|end  ← Pomodoro tracking
│   │   │   │   ├── auth/
│   │   │   │   │   └── signin/   page.tsx
│   │   │   │   └── dashboard/
│   │   │   │       ├── page.tsx         ← Dashboard
│   │   │   │       ├── chat/page.tsx    ← AI Chat
│   │   │   │       ├── upload/page.tsx  ← File upload
│   │   │   │       ├── quiz/page.tsx    ← Quiz engine
│   │   │   │       ├── planner/page.tsx ← Study planner
│   │   │   │       ├── focus/page.tsx   ← Pomodoro timer
│   │   │   │       └── progress/page.tsx← Analytics
│   │   │   ├── components/
│   │   │   │   ├── layout/       sidebar.tsx · top-nav.tsx
│   │   │   │   ├── features/     dashboard/ · chat/ · quiz/
│   │   │   │   └── providers.tsx ← Auth · Theme · React Query
│   │   │   ├── hooks/
│   │   │   │   ├── use-documents.ts
│   │   │   │   └── use-focus-session.ts
│   │   │   └── lib/
│   │   │       ├── auth.ts            ← NextAuth v5 config
│   │   │       ├── db.ts              ← Prisma singleton
│   │   │       ├── vector-store.ts    ← ChromaDB client
│   │   │       ├── document-processor.ts ← RAG pipeline
│   │   │       ├── chat-service.ts    ← Chat history helpers
│   │   │       └── storage.ts         ← S3 / local file storage
│   │   ├── prisma/
│   │   │   └── schema.prisma     ← Full DB schema
│   │   ├── Dockerfile
│   │   └── package.json
│   │
│   └── api/                      # FastAPI Python backend
│       ├── main.py               ← App entry + CORS + routers
│       ├── core/config.py        ← Pydantic settings
│       ├── routers/              ← documents · quiz · planner · analytics
│       ├── services/             ← AI service layer
│       ├── requirements.txt
│       └── Dockerfile
│
├── packages/
│   ├── types/src/index.ts        ← Shared TypeScript types
│   ├── ui/                       ← Shared component library
│   └── ai/                       ← Shared AI utilities
│
├── docker-compose.yml            ← Full stack: PG + Chroma + Redis + API + Web
├── turbo.json                    ← Turborepo pipeline config
├── package.json                  ← Workspace root
└── .env.example                  ← All required env vars documented
```

---

## 🗄 Database Schema

Built with **Prisma ORM** on **PostgreSQL 16**. Key models:

```prisma
User              # Auth — NextAuth adapter compatible
├── Account       # OAuth providers (Google, GitHub)
├── Session       # JWT sessions
├── Document      # Uploaded study files
│   └── DocumentChunk   # RAG text chunks + ChromaDB vector IDs
├── ChatSession   # Conversation threads
│   └── ChatMessage     # Messages with source citations
├── QuizSession   # Generated quizzes
│   └── Question        # Individual questions with explanations
├── FlashcardSet  # Flashcard decks
│   └── Flashcard       # SM-2 spaced repetition cards
├── StudyPlan     # AI-generated schedules
│   └── PlannedSession  # Individual study sessions
├── FocusSession  # Pomodoro timer records
├── Subject       # Student's subjects
├── Exam          # Upcoming exam dates
└── StudyStats    # Aggregated analytics (streak, hours, scores)
```

Full schema → [`apps/web/prisma/schema.prisma`](apps/web/prisma/schema.prisma)

---

## 🤖 RAG Pipeline

How your documents become intelligent answers:

```
1. UPLOAD
   User uploads PDF / DOCX / TXT
        │
        ▼
2. EXTRACT
   LangChain Loaders (PDFLoader, DocxLoader, TextLoader)
   → Raw text extracted with page metadata
        │
        ▼
3. CHUNK
   RecursiveCharacterTextSplitter
   chunk_size=1000, overlap=200
   → Semantically meaningful text chunks
        │
        ▼
4. EMBED
   OpenAI text-embedding-3-small
   → 1536-dimensional vectors per chunk
        │
        ▼
5. STORE
   ChromaDB (cosine similarity index)   ← Vector search
   PostgreSQL DocumentChunk table       ← Metadata + content
        │
        ▼
6. QUERY (at chat time)
   User question → embed → ChromaDB similarity search
   → Top-K relevant chunks retrieved
        │
        ▼
7. GENERATE
   GPT-4o + retrieved context + chat history
   → Streaming, cited response via Vercel AI SDK
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** ≥ 20 — [Download](https://nodejs.org)
- **pnpm** ≥ 9 — `npm install -g pnpm`
- **Python** ≥ 3.12 — [Download](https://python.org)
- **Docker & Docker Compose** — [Download](https://docker.com)
- **OpenAI API Key** — [Get one](https://platform.openai.com)

### 1 — Clone the repository

```bash
git clone https://github.com/your-username/studyai.git
cd studyai
```

### 2 — Install dependencies

```bash
pnpm install
```

### 3 — Configure environment variables

```bash
cp .env.example apps/web/.env.local
```

Open `apps/web/.env.local` and fill in:

```env
# Required
DATABASE_URL="postgresql://studyai:studyai_password@localhost:5432/studyai_db"
OPENAI_API_KEY=sk-your-key-here
NEXTAUTH_SECRET=your-secret-min-32-chars   # openssl rand -base64 32
NEXTAUTH_URL=http://localhost:3000

# Optional (OAuth)
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...
GITHUB_CLIENT_ID=...
GITHUB_CLIENT_SECRET=...
```

### 4 — Start infrastructure

```bash
docker-compose up -d postgres chromadb redis
```

This starts:
- **PostgreSQL** on `localhost:5432`
- **ChromaDB** on `localhost:8001`
- **Redis** on `localhost:6379`

### 5 — Set up the database

```bash
cd apps/web
npx prisma migrate dev --name init
npx prisma generate
cd ../..
```

### 6 — Start development servers

```bash
# Starts all apps via Turborepo
pnpm dev
```

Or individually:

```bash
# Terminal 1 — Next.js frontend
cd apps/web && pnpm dev          # http://localhost:3000

# Terminal 2 — FastAPI backend
cd apps/api
pip install -r requirements.txt
uvicorn main:app --reload        # http://localhost:8000
```

### 7 — Open the app

Visit **http://localhost:3000** and sign up for an account.

---

## ⚙️ Environment Variables

All variables are documented in [`.env.example`](.env.example).

| Variable | Required | Description |
|---|---|---|
| `DATABASE_URL` | ✅ | PostgreSQL connection string |
| `OPENAI_API_KEY` | ✅ | OpenAI API key (GPT-4o + embeddings) |
| `NEXTAUTH_SECRET` | ✅ | Random secret ≥ 32 characters |
| `NEXTAUTH_URL` | ✅ | App URL (`http://localhost:3000` in dev) |
| `CHROMA_HOST` | ✅ | ChromaDB URL (`http://localhost:8001`) |
| `GOOGLE_CLIENT_ID` | ☑️ | Google OAuth (optional) |
| `GITHUB_CLIENT_ID` | ☑️ | GitHub OAuth (optional) |
| `AWS_S3_BUCKET` | ☑️ | S3 bucket for file storage (optional, uses local in dev) |
| `REDIS_URL` | ☑️ | Redis URL for caching (optional) |

---

## 🔌 API Reference

### Next.js API Routes

| Method | Route | Description |
|---|---|---|
| `POST` | `/api/auth/[...nextauth]` | NextAuth authentication handlers |
| `GET` | `/api/documents` | List all user documents |
| `POST` | `/api/documents` | Upload a new document |
| `GET` | `/api/documents/:id` | Get document details & status |
| `DELETE` | `/api/documents/:id` | Delete document + vectors |
| `POST` | `/api/chat` | **Streaming** RAG chat response |
| `POST` | `/api/quiz/generate` | Generate quiz from document with AI |
| `POST` | `/api/quiz/:id/submit` | Submit quiz answers + calculate score |
| `POST` | `/api/planner/generate` | Generate AI study schedule |
| `POST` | `/api/focus/start` | Start a Pomodoro session |
| `POST` | `/api/focus/end` | End session + update study stats |

### FastAPI Routes

| Method | Route | Description |
|---|---|---|
| `GET` | `/health` | Health check |
| `POST` | `/api/documents/process` | Background document processing |
| `GET` | `/api/analytics/summary` | Aggregated study analytics |
| `POST` | `/api/search/semantic` | Semantic search across all documents |

Full API docs available at `http://localhost:8000/docs` in development.

---

## 🐳 Docker Deployment

### Full stack with one command

```bash
# Copy and configure environment
cp .env.example .env
# Edit .env with your values

# Build and start everything
docker-compose up --build -d

# Check all services are running
docker-compose ps

# View logs
docker-compose logs -f api
docker-compose logs -f web
```

Services started:

| Service | Port | Description |
|---|---|---|
| `web` | 3000 | Next.js frontend |
| `api` | 8000 | FastAPI backend |
| `postgres` | 5432 | PostgreSQL database |
| `chromadb` | 8001 | Vector database |
| `redis` | 6379 | Cache / sessions |

### Useful commands

```bash
docker-compose down              # Stop all services
docker-compose down -v           # Stop + delete volumes
docker-compose logs -f           # Follow all logs
docker-compose exec api bash     # Shell into API container
docker-compose exec postgres psql -U studyai studyai_db
```

---

## 🚢 Production Deployment

### Frontend → Vercel

```bash
cd apps/web

# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

Set all environment variables in the Vercel dashboard under **Settings → Environment Variables**.

### Backend → Railway / Render / Fly.io

#### Railway (recommended)

```bash
# Install Railway CLI
npm i -g @railway/cli

cd apps/api
railway login
railway init
railway up
```

#### Docker (any VPS / AWS ECS / GCP Cloud Run)

```bash
# Build API image
docker build -t studyai-api ./apps/api

# Push to registry
docker tag studyai-api your-registry/studyai-api:latest
docker push your-registry/studyai-api:latest
```

### Database → Supabase / Neon (managed PostgreSQL)

```bash
# After setting DATABASE_URL to your managed DB:
cd apps/web
npx prisma migrate deploy
```

---

## 🛠 Tech Stack

### Frontend (`apps/web`)

| Technology | Version | Purpose |
|---|---|---|
| [Next.js](https://nextjs.org) | 15 | React framework with App Router |
| [TypeScript](https://typescriptlang.org) | 5.4 | Type safety |
| [Tailwind CSS](https://tailwindcss.com) | 3.4 | Utility-first styling |
| [shadcn/ui](https://ui.shadcn.com) | latest | Accessible UI components |
| [Framer Motion](https://framer.com/motion) | 11 | Animations |
| [Vercel AI SDK](https://sdk.vercel.ai) | 3 | Streaming AI responses |
| [LangChain](https://js.langchain.com) | 0.2 | RAG pipeline |
| [Prisma](https://prisma.io) | 5 | Database ORM |
| [NextAuth v5](https://authjs.dev) | 5 beta | Authentication |
| [TanStack Query](https://tanstack.com/query) | 5 | Server state management |
| [Zustand](https://zustand-demo.pmnd.rs) | 4 | Client state management |
| [Recharts](https://recharts.org) | 2 | Analytics charts |
| [Sonner](https://sonner.emilkowal.ski) | 1 | Toast notifications |
| [react-dropzone](https://react-dropzone.js.org) | 14 | File upload |
| [react-markdown](https://github.com/remarkjs/react-markdown) | 9 | Markdown rendering |
| [Zod](https://zod.dev) | 3 | Schema validation |

### Backend (`apps/api`)

| Technology | Version | Purpose |
|---|---|---|
| [FastAPI](https://fastapi.tiangolo.com) | 0.111 | Python API framework |
| [LangChain](https://python.langchain.com) | 0.2 | AI orchestration |
| [OpenAI SDK](https://platform.openai.com/docs) | 1.35 | GPT-4o + embeddings |
| [ChromaDB](https://trychroma.com) | 0.5 | Vector database |
| [SQLAlchemy](https://sqlalchemy.org) | 2 | Async ORM |
| [Pydantic](https://docs.pydantic.dev) | 2 | Data validation |
| [PyPDF](https://pypdf.readthedocs.io) | 4 | PDF text extraction |
| [python-docx](https://python-docx.readthedocs.io) | 1.1 | DOCX processing |
| [Redis](https://redis.io) | 7 | Caching |
| [uvicorn](https://uvicorn.org) | 0.30 | ASGI server |

### Infrastructure

| Technology | Purpose |
|---|---|
| PostgreSQL 16 | Primary relational database |
| ChromaDB | Vector embeddings storage |
| Redis 7 | Caching, rate limiting |
| Docker + Compose | Containerization |
| AWS S3 | Production file storage |
| Turborepo | Monorepo build system |

---

## 🤝 Contributing

Contributions are what make the open-source community amazing. Any contributions you make are **greatly appreciated**.

### How to contribute

1. **Fork** the repository
2. **Create** a feature branch
   ```bash
   git checkout -b feat/amazing-feature
   ```
3. **Commit** your changes using [Conventional Commits](https://conventionalcommits.org)
   ```bash
   git commit -m "feat: add flashcard export to PDF"
   ```
4. **Push** to your branch
   ```bash
   git push origin feat/amazing-feature
   ```
5. **Open** a Pull Request

### Development commands

```bash
pnpm dev           # Start all apps in dev mode
pnpm build         # Build all apps
pnpm lint          # Run ESLint across all packages
pnpm type-check    # TypeScript checks
pnpm test          # Run all tests
pnpm db:studio     # Open Prisma Studio (database GUI)
pnpm db:migrate    # Run database migrations
pnpm db:generate   # Regenerate Prisma client
```

### Good first issues

- 🌐 Add more OAuth providers (Discord, Twitter)
- 📱 Improve mobile responsiveness
- 🔍 Add full-text search across documents
- 🎨 Create additional UI themes
- 🌍 Add internationalisation (i18n) support
- 📊 Add more analytics chart types
- 🧪 Add unit and integration tests

---

## 📄 License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for more information.

---

## 🙏 Acknowledgements

- [OpenAI](https://openai.com) — GPT-4o and embedding models
- [LangChain](https://langchain.com) — RAG framework
- [ChromaDB](https://trychroma.com) — Vector database
- [shadcn/ui](https://ui.shadcn.com) — UI component system
- [Vercel](https://vercel.com) — Hosting and AI SDK
- [Prisma](https://prisma.io) — Database toolkit

---

<div align="center">

<br/>

Built with ❤️ for students everywhere

**Study smart. Study AI.**

<br/>

⭐ **Star this repo** if StudyAI helped you — it means a lot!

<br/>

[🐛 Report Bug](../../issues/new?template=bug_report.md) · [✨ Request Feature](../../issues/new?template=feature_request.md) · [💬 Discussions](../../discussions)

</div>
