# BAC/BEM Exam Prep Platform — Part I: Project Overview

**Project key:** `BACBEM` · **Compiled:** 2026-08-12
**Source:** consolidated from `claude.md`, `map.md`, `doc.md`, `main.md`, `v1.md`,
`v1_1.md`, `report.md`, `plan.md`, `tech.md`, `colors.md`, `postgres.md`, `redis.md`.

**This is Part I of III.** Part II covers the full Jira-style backlog (24 epics, 65+
stories, 185+ QA test cases). Part III covers the same core + new capabilities written
out as full narrative user stories (`As a / I want / So that` + Given/When/Then).

---

# BAC/BEM Exam Prep Platform — Master Document

**Compiled from:** `claude.md`, `map.md`, `doc.md`, `main.md`, `v1.md`, `v1_1.md`,
`report.md`, `plan.md`, `tech.md`, `colors.md`, `postgres.md`, `redis.md`
**Compiled on:** 2026-08-12
**Status of source docs:** `report.md` (dated 2026-08-11) is the most authoritative
description of *what currently runs*. `plan.md` is the next implementation pass on top
of it. `claude.md` and `v1.md` are earlier/larger target specs that were partially
superseded by a product-direction change on 2026-08-09. This document reconciles all of
them into one narrative and adds detailed user stories for every major capability.

---

## 1. What this platform is

A bilingual (Arabic/English, French content) web platform that helps two cohorts of
Algerian students prepare for their national final exams:

- **BAC** candidates — *Terminale*, last year of high school
- **BEM** candidates — *4ème moyenne*, final year of middle school

Students work inside subject-scoped **spaces** (e.g. "Math") built from reusable
**widgets**:

- **Chatbot** — general AI tutor chat
- **Assisting** — homework-help chat (same chat mechanics, different system prompt)
- **Exercising** — AI-generated practice problems with a self-graded reveal-solution step
- **Learning** — a status list of the space's course-material library (PDFs), never
  served as raw downloadable files to students

Every AI chat answer and every generated exercise is **grounded in a retrieval-augmented
(RAG) knowledge base** built from real, admin-uploaded course PDFs, so responses cite
actual syllabus content instead of being generic model output.

Underneath the widgets sits a **personalization engine**: every self-rated exercise
attempt updates a per-topic mastery estimate for that student. That estimate — not
randomness — decides what topic and difficulty the student sees next, and rolls up into
a BAC/BEM-readiness score that a linked **parent** account can track per subject and per
topic, including a "needs attention" flag and an anonymized cohort-average comparison.

An **admin/teacher** panel configures spaces, the widget catalog (including which local
LLM backs each widget), the document corpus, the syllabus topic taxonomy, groups, users,
and parent↔child links — all without code changes.

### Full feature set (core learning + standard product surface)

The core learning loop above (spaces, widgets, RAG, personalization, parent analytics,
admin panel) is documented in depth throughout this document and in Parts II/III. A
product at this stage of maturity is also expected to ship the same supporting surface
any mainstream Western SaaS/edtech product ships, so a student, parent, or admin never
hits an obviously-missing wall. This platform's target feature set therefore adds:

- **Onboarding & guided setup** — a short wizard on first login (track, exam year,
  subjects, daily goal, notification preferences) instead of dropping a new student on
  an empty dashboard.
- **Notifications center** — an in-app bell, email digests, and optional push,
  covering streak risk, topics due for review, parent-visible milestones, and
  admin/system announcements.
- **Gamification & achievements** — XP, badges, and an opt-in leaderboard scoped to a
  student's own groups (never a raw global ranking that could demotivate).
- **Global search** — one search bar across spaces, topics, and past chat/exercise
  history.
- **Help center, support chat & feedback** — self-serve FAQ/docs, a lightweight
  support widget, and a thumbs-up/down + comment on every AI response.
- **User profile & settings** — avatar, notification preferences, accessibility
  settings (font size, reduced motion, contrast), and a security tab (password,
  sessions, 2FA).
- **Pricing, subscription & referral** — a public pricing page, an in-app upgrade
  flow, and a "refer a friend" program with tracked credit.
- **Legal & compliance** — cookie consent banner, Terms of Service, Privacy Policy,
  and a GDPR-style "download/delete my data" flow.
- **Study planner & calendar integration** — a weekly planner view with optional
  Google Calendar / iCal export of study sessions and the exam date.
- **Community / study groups** — space-scoped discussion threads so students in the
  same group can ask each other (not just the AI) for help, moderated by teachers.
- **Changelog / "what's new"** — a lightweight release-notes surface so returning
  users see the product is actively improving.

These are covered as EPIC-15 through EPIC-24 in Part II, with narrative user stories
for the highest-priority ones in Part III.

### Product direction history (why some docs disagree with each other)
1. **Original spec (`claude.md`)** — a large subjects → topics → exercises content
   hierarchy with a pluggable AI provider registry (`mock` / `heuristic` / `sklearn` /
   `llm`), population-level "common mistake" mining, and a full Docker Compose stack.
2. **`map.md`** — scoped `claude.md` down to a no-Docker, phase 0–4 MVP for local dev,
   keeping the AI-gateway seam and schema intent intact.
3. **2026-08-09 pivot (`doc.md`, `main.md`)** — the product direction changed to the
   simpler **spaces + widgets** model described above, built directly rather than
   through the original phase list. The subjects/topics/exercises content hierarchy and
   the mastery/mistake-mining pieces from `claude.md` became historical/aspirational.
4. **RAG-driven blueprint (`v1.md`, `v1_1.md`)** — the spaces/widgets model was extended
   with a real retrieval-augmented generation pipeline (PDF ingestion, embeddings,
   citation-grounded chat/exercises), a syllabus-topic personalization engine, parent
   analytics, organizations/billing concepts, and a full operational + design-system
   companion spec.
5. **Current state (`report.md`, 2026-08-11)** — documents what of the `v1.md`/`v1_1.md`
   blueprint is actually implemented and verified end-to-end today, and what remains a
   gap.
6. **`plan.md`** — the next implementation pass (operational plumbing + design system
   tasks) building directly on the state described in `report.md`.

**What survived every iteration unchanged:** the AI-gateway seam (nothing outside
`app/services/llm_gateway.py` / `app/ai/registry.py` talks to an LLM provider directly),
hand-rolled JWT auth, and the calm/competent visual language for a teenage,
exam-pressure audience.

---

## 2. Tech stack (current, per `report.md`/`tech.md`)

| Layer | Choice | Notes |
|---|---|---|
| Language | Python 3.12+ | |
| Web framework | FastAPI (async) | |
| Server | Uvicorn via `python app.py` | no Docker/Gunicorn for local dev; `reload=True` |
| Database | PostgreSQL (local instance v18) | `pgcrypto` installed; `pgvector` **not** installed |
| ORM | SQLAlchemy 2.0 (async, `asyncpg`) + Alembic | |
| Validation / config | Pydantic v2 / `pydantic-settings` | `app/config.py`, reads `.env` |
| Auth | Hand-rolled JWT, httponly session cookie | `python-jose` + `passlib[bcrypt]`; see §7 gaps |
| Templates | Jinja2 + HTMX (partial swaps) + Alpine.js | no SPA build step |
| Styling | Hand-written CSS design tokens (`app/static/css/theme.css`) | Tailwind configured but not the primary path |
| LLM integration | `httpx` to any OpenAI-compatible local server (Ollama, LM Studio, LocalAI) | configured **per widget** from the admin panel |
| Embeddings | Same per-widget/per-space config pattern | needs a multilingual (AR/FR/EN) model for cross-lingual retrieval |
| Text extraction / OCR | `pdfplumber` + `pytesseract` OCR fallback | OCR requires the `tesseract` binary on the host (not installed on the dev machine — graceful degradation verified) |
| Vector search | Python/NumPy cosine similarity (`app/rag/retriever.py`) | fallback because `pgvector` isn't available; isolated behind one module for an easy swap |
| Background jobs | Celery, broker/backend = Redis | 2-second enqueue attempt, inline fallback if no broker reachable |
| ML | scikit-learn, pandas (dependencies only) | for a future trained personalization model; nothing running today |
| Testing | pytest, pytest-asyncio, httpx | scaffolded, not fully populated |
| Containerization | `docker-compose.yml` + `Dockerfile` in repo | unused for local dev |

### Additional infrastructure for the full feature set (EPIC-15 → EPIC-24, planned)

| Layer | Choice | Notes |
|---|---|---|
| Transactional email | Postmark or SendGrid | onboarding, notification digests, receipts |
| Push notifications | Web Push (VAPID) | opt-in only, browser-native, no third-party SDK required |
| Full-text search | Postgres `tsvector`/`pg_trgm` initially; Meilisearch if latency demands it | scoped to spaces/topics/chat history the requester can already see — search never bypasses existing access control |
| Payments/subscription | Stripe (or a regional processor if Stripe isn't available in-market) | pricing page + in-app upgrade flow |
| Calendar export | `icalendar` (Python) for `.ics`; Google Calendar API for direct sync | study planner export |
| Feature flags | `.env` (Tier-1) + `feature_flags` DB table (Tier-2) | see EPIC-11 in the backlog |
| Analytics events | see EPIC-13 in the backlog | category-separated stores, not a single firehose table |

---

## 3. Architecture

```
Browser (HTMX, Jinja2, RTL/LTR-aware)
        │
        ▼
FastAPI app (app/main.py)
  ├─ routers/pages/auth.py        login/logout, language change
  ├─ routers/pages/dashboard.py   space list + readiness badges
  ├─ routers/pages/spaces.py      space page + widget actions (chat/exercise/rate)
  ├─ routers/pages/parent.py      child analytics + language control
  └─ routers/pages/admin.py       spaces/widgets/groups/users/corpus/topics
        │
        ▼
services/  (business logic — routers never touch providers/retrieval directly)
  ├─ auth_service, space_service, parent_service, analytics_service
  ├─ recommendation_service   *** personalization seam ***
  ├─ mastery_service          *** mastery update seam ***
  ├─ rag_service               *** retrieval seam ***
  ├─ ingestion_service          corpus upload orchestration
  └─ llm_gateway                *** generation seam ***
        │
        ├───────────────────────────────┐
        ▼                                ▼
rag/retriever.py (cosine similarity)   ai/providers/local_llm_provider.py
        │                                │
        ▼                                ▼
PostgreSQL (SQLAlchemy async)          any OpenAI-compatible local LLM server
        ▲
        │
worker/  (Celery — real when a broker is reachable, inline fallback otherwise)
  ├─ ingestion_tasks.py   PDF → text → chunks → embeddings
  └─ mastery_tasks.py     mastery recompute after a rated attempt
```

**Non-negotiable architectural rule:** no router or template ever calls
`app/ai/providers/*` or `app/rag/retriever.py` directly. Everything routes through
`llm_gateway.py` and `rag_service.py` — this is what lets a widget's model and a space's
retrieval settings change from the admin panel with zero code changes.

### Repo layout (current)
```
app/
  main.py, config.py
  core/        security.py, permissions.py, deps.py, templates.py, i18n.py
  db/          base.py, deps.py, seed.py
  models/      user, space, chat, attempt, topic, document
  services/    auth, space, parent, analytics, recommendation, mastery, rag,
               ingestion, llm_gateway
  rag/         chunker.py, embedder.py, retriever.py
  ai/          providers/local_llm_provider.py
  routers/pages/  auth, dashboard, spaces, parent, admin
  templates/   base.html, dashboard.html, login.html, space.html,
               admin/*, parent/*, partials/*
  static/css/  theme.css
  worker/      celery_app.py, ingestion_tasks.py, mastery_tasks.py
alembic/versions/
math/          seeded course PDFs (ingested into RAG, never served raw)
app.py
```

---

## 4. Roles

| Role | Summary |
|---|---|
| **Student** | Practices inside spaces/widgets their groups grant access to; gets AI tutoring, homework help, and self-graded practice exercises grounded in real course material. |
| **Parent** | Read-only view of linked children's readiness score, per-topic mastery, cohort comparison, and activity; can lock/unlock the child's UI language. |
| **Teacher / Admin** | Configures spaces, widgets (incl. LLM connection), corpus, syllabus topics, groups, users, and parent↔child links. |

---

## 5. Design system

Palette source: `colors.md`.

| Token | Light | Dark |
|---|---|---|
| `--color-bg` | `#e2e8e2` | `#12261f` |
| `--color-surface` | `#ffffff` | `#0a4530` |
| `--color-surface-raised` | `#ffffff` | `#0f5b3e` |
| `--color-teal` | `#0f6b52` | `#0f6b52` |
| `--color-text` | `#12261f` | `#ffffff` |
| `--color-muted` | `#5a6b62` | `#93a69c` (derived) |
| `--color-gold` | `#966f1f` (derived) | `#b8912f` |
| `--color-gold-light` | `#b8912f` | `#d9b968` |
| `--color-success` | `#0f6b52` | `#3fae86` (derived) |

**Direction:** a "focused study-room" identity — deep ink/navy dark mode for late-night
study (dark mode is the implied default), a warm gold accent reserved *only* for
progress/success moments (streaks, mastery gains, the readiness ring), calm/direct copy
with no exclamation marks in system text, and a geometric display face for numbers
paired with an Arabic-supporting body face (Cairo / IBM Plex Sans Arabic). Full RTL/LTR
support is driven by `dir="rtl"|"ltr"` on `<html>` from the active locale — not
optional, not retrofitted.

---

## 6. Local setup

1. PostgreSQL running and reachable, `pgcrypto` extension available (`pgvector` is
   **not** required — retrieval falls back to Python/NumPy cosine similarity).
   Credentials for this dev machine: see `postgres.md` (`localhost:5432`).
2. Redis: `docker run -d --name redis -p 6379:6379 redis` (see `redis.md`) — optional,
   Celery falls back to inline execution if unreachable.
3. Copy `.env.example` → `.env`, set `DATABASE_URL` / `REDIS_URL`.
4. `pip install -e ".[dev]"`
5. `python -m alembic upgrade head`
6. `python -m app.db.seed` — idempotent; creates admin/student/parent accounts, a
   "Math" space + all widget types, syllabus topics, and ingests seed PDFs into the RAG
   corpus. Writes `creds.txt` (gitignored) at the project root.
7. `python app.py` → `http://127.0.0.1:8000` (reload on).

---

## 7. Known gaps (honest current state, per `report.md` §7)

- Session handling is a single 12-hour cookie, not real access+refresh (the refresh
  code exists but is dead/unused).
- No rate limiting anywhere (login, chat, exercise generation all unlimited).
- No server-level feature-flag kill switch ahead of the admin panel.
- No usage-time tracking beyond discrete timestamped events.
- No analytics/event pipeline — everything is computed on read from transactional tables.
- No billing/monetization primitives (no balance, transaction, or paywall concept).
- No minimum-n guard on the parent-facing cohort-average comparison (small cohorts could
  be de-anonymized by inference) — closed by `plan.md`'s `ANALYTICS_COHORT_MIN_N`.

---

