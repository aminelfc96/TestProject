# Source Digest — BAC/BEM Exam Prep Platform (Part I)

**Purpose:** A single authoritative digest of source document D1 (Part I of III) so the product vision, requirements, architecture, security and backlog steps can proceed from one faithful summary. It records everything material D1 states and flags precisely what it does not state.

## 1. Overview

- **Documents:** D1 consolidates twelve sources — claude.md, map.md, doc.md, main.md, v1.md, v1_1.md, report.md, plan.md, tech.md, colors.md, postgres.md, redis.md (SRC-D1-§1, SRC-D1-§2).
- **Purpose:** One reconciled narrative of the BAC/BEM exam-prep platform: what it is, what currently runs, and the planned feature set (SRC-D1-§2).
- **Audience:** The delivery team and the Super Admin, for readers who need a complete but non-duplicative picture (SRC-D1-§2).
- **Authority:** report.md (2026-08-11) is the most authoritative description of what currently runs; plan.md is the next implementation pass; claude.md and v1.md are earlier/larger specs partially superseded by the 2026-08-09 product-direction change (SRC-D1-§2.1.2, SRC-D1-§2.7).
- **Scope of this digest:** D1 is Part I of III. Part II (backlog: 24 epics, 65+ stories, 185+ QA cases) and Part III (narrative user stories) are referenced but not supplied — see ASM-001.

## 2. Structure table

| Section ID | Title | Summary |
|---|---|---|
| SRC-D1-§1 | BAC/BEM Exam Prep Platform — Part I: Project Overview | Part I of III; reconciles 12 source docs into one narrative. Parts II (backlog) and III (narrative stories) follow. |
| SRC-D1-§2 | Master Document | Compilation header; states report.md is authoritative for current state and plan.md is the next pass. |
| SRC-D1-§2.1 | What this platform is | Bilingual AR/EN platform (French content) for BAC and BEM students; subject-scoped spaces of widgets; RAG-grounded chat/exercises; mastery personalization; parent analytics; admin panel. |
| SRC-D1-§2.1.1 | Full feature set | Standard SaaS/edtech surface (onboarding, notifications, gamification, search, help, profile, pricing, legal, planner, community, changelog) as EPIC-15→24. |
| SRC-D1-§2.1.2 | Product direction history | Six-stage evolution claude.md→plan.md; surviving invariants: AI-gateway seam, hand-rolled JWT, calm visual language. |
| SRC-D1-§2.2 | Tech stack (current) | Python 3.12+/FastAPI/SQLAlchemy async/PostgreSQL 18 (pgcrypto, no pgvector)/Celery+Redis/Jinja2+HTMX+Alpine/NumPy cosine RAG/llm gateway to OpenAI-compatible local servers. |
| SRC-D1-§2.2.1 | Additional infrastructure (planned) | Email, Web Push, full-text search, payments, calendar export, feature flags, analytics events for EPIC-15→24. |
| SRC-D1-§2.3 | Architecture | Browser → FastAPI routers → services → rag/retriever + ai/providers; Celery workers; rule that routers/templates never touch providers/retriever directly. |
| SRC-D1-§2.3.1 | Repo layout | app/ tree (routers, services, models, rag, ai, templates, static, worker), alembic, math/ seeded PDFs, app.py. |
| SRC-D1-§2.4 | Roles | Student (practices in granted spaces), Parent (read-only analytics + language lock), Teacher/Admin (configures everything). |
| SRC-D1-§2.5 | Design system | Color tokens light/dark; "focused study-room" identity; dark implied default; gold reserved for progress; Cairo/IBM Plex Sans Arabic; full RTL/LTR. |
| SRC-D1-§2.6 | Local setup | 7-step dev setup: Postgres+pgcrypto, optional Redis, .env, pip install, alembic, idempotent seed (creds.txt), run app.py. |
| SRC-D1-§2.7 | Known gaps | 12-hour-only session, no rate limiting, no feature-flag kill switch, no usage-time tracking, no analytics pipeline, no billing, no cohort min-n guard. |

## 3. Key facts

### 3.1 Stakeholders and users

| Stakeholder | Role | Source |
|---|---|---|
| Student — BAC candidate | Terminale (last year of high school); prepares for the BAC. | SRC-D1-§2.1, §2.4 |
| Student — BEM candidate | 4ème moyenne (final year of middle school); prepares for the BEM. | SRC-D1-§2.1, §2.4 |
| Parent | Read-only view of linked children's readiness/mastery/cohort comparison/activity; locks or unlocks the child's UI language. | SRC-D1-§2.1, §2.4 |
| Teacher / Admin | Configures spaces, widgets (incl. LLM), corpus, syllabus topics, groups, users, parent↔child links — no code. | SRC-D1-§2.1, §2.4 |

### 3.2 Business goals

- Help Algerian students prepare for the national BAC and BEM exams through subject-scoped practice and AI tutoring (SRC-D1-§2.1).
- Make AI answers and exercises trustworthy by grounding them in real, admin-uploaded course PDFs and citing actual syllabus content (SRC-D1-§2.1).
- Personalize learning from each self-rated attempt, and give parents visibility into readiness with a needs-attention flag and cohort comparison (SRC-D1-§2.1).
- Make the platform fully configurable without code changes (SRC-D1-§2.1).
- Ship a mainstream SaaS/edtech surface and monetize via pricing/subscription/referral (planned) (SRC-D1-§2.1.1, §2.2.1).

### 3.3 Constraints

- **Regulatory/legal (planned):** cookie consent banner, Terms of Service, Privacy Policy, GDPR-style download/delete-my-data flow (SRC-D1-§2.1.1). See Q-003 on jurisdiction.
- **Technical:** Python 3.12+; FastAPI async; PostgreSQL 18 local (pgcrypto yes, pgvector no); Redis optional; NumPy cosine retrieval; Celery with inline fallback; no Docker/Gunicorn for local dev (SRC-D1-§2.2).
- **Architectural:** no router or template may call app/ai/providers/* or app/rag/retriever.py directly (SRC-D1-§2.3).
- **Design:** calm "focused study-room" identity; dark mode implied default; gold reserved for progress/success; no exclamation marks in system text; full RTL/LTR (SRC-D1-§2.5).
- **Organizational:** conflicting historical docs reconciled by a stated authority order (SRC-D1-§2.1.2, §2.7).

Repo layout (current): app/ (main.py, config.py, core/, db/, models/, services/, rag/, ai/providers/, routers/pages/, templates/, static/css/, worker/), alembic/versions/, math/ (seeded course PDFs), app.py (SRC-D1-§2.3.1).

## 4. Explicit requirement statements

These statements are paraphrased faithfully from D1 and are section-traceable; they will be formalized as REQ-FUN-*/REQ-NFR-* items in the requirements step.

### 4.1 Core learning loop (SRC-D1-§2.1)

- R1. The platform must be bilingual (Arabic/English) with French content.
- R2. It must serve two cohorts: BAC candidates (Terminale) and BEM candidates (4ème moyenne).
- R3. Students must work inside subject-scoped spaces (e.g. "Math") built from reusable widgets.
- R4. The widget catalog must include Chatbot (general AI tutor chat), Assisting (homework-help chat, same mechanics, different system prompt), Exercising (AI-generated practice problems with a self-graded reveal-solution step), and Learning (status list of the space's course-material PDF library).
- R5. The Learning widget must never serve course PDFs as raw downloadable files.
- R6. Every AI chat answer and generated exercise must be grounded in a RAG knowledge base built from admin-uploaded course PDFs and must cite actual syllabus content, not generic model output.
- R7. Every self-rated exercise attempt must update a per-topic mastery estimate for that student.
- R8. The mastery estimate — not randomness — must decide the next topic and difficulty.
- R9. Mastery estimates must roll up into a BAC/BEM-readiness score.
- R10. A linked parent account must be able to track the child's readiness per subject and per topic.
- R11. Parent tracking must include a "needs attention" flag and an anonymized cohort-average comparison.
- R12. An admin/teacher panel must configure spaces, the widget catalog (including which local LLM backs each widget), the document corpus, the syllabus topic taxonomy, groups, users, and parent↔child links — without code changes.

### 4.2 Full feature set / standard product surface (SRC-D1-§2.1.1, planned as EPIC-15→24)

- R13. Onboarding: a short first-login wizard for track, exam year, subjects, daily goal, and notification preferences.
- R14. Notifications center: in-app bell, email digests, and optional push, covering streak risk, topics due for review, parent-visible milestones, and admin/system announcements.
- R15. Gamification: XP, badges, and an opt-in leaderboard scoped to the student's own groups — never a raw global ranking.
- R16. Global search: one search bar across spaces, topics, and past chat/exercise history.
- R17. Help center, support chat & feedback: self-serve FAQ/docs, a lightweight support widget, and thumbs-up/down plus comment on every AI response.
- R18. User profile & settings: avatar, notification preferences, accessibility settings (font size, reduced motion, contrast), and a security tab (password, sessions, 2FA).
- R19. Pricing, subscription & referral: a public pricing page, an in-app upgrade flow, and a refer-a-friend program with tracked credit.
- R20. Legal & compliance: cookie consent banner, Terms of Service, Privacy Policy, and a GDPR-style download/delete-my-data flow.
- R21. Study planner & calendar: a weekly planner view with optional Google Calendar / iCal export of study sessions and the exam date.
- R22. Community/study groups: space-scoped discussion threads moderated by teachers.
- R23. Changelog / "what's new": a lightweight release-notes surface.

### 4.3 Architecture and technical (SRC-D1-§2.2, §2.2.1, §2.3)

- R24. Runtime: Python 3.12+, FastAPI (async), Uvicorn via python app.py with reload=True for local dev (no Docker/Gunicorn locally).
- R25. Database: PostgreSQL (local instance v18) with pgcrypto installed; pgvector is not installed.
- R26. ORM/validation: SQLAlchemy 2.0 (async, asyncpg) + Alembic; Pydantic v2 / pydantic-settings reading .env.
- R27. Auth: hand-rolled JWT delivered in an httponly session cookie (python-jose + passlib[bcrypt]).
- R28. Frontend: Jinja2 + HTMX partial swaps + Alpine.js; no SPA build step; hand-written CSS design tokens (theme.css), Tailwind configured but not the primary path.
- R29. LLM integration: httpx to any OpenAI-compatible local server (Ollama, LM Studio, LocalAI), configured per widget from the admin panel.
- R30. Embeddings: same per-widget/per-space config pattern; must use a multilingual (AR/FR/EN) model for cross-lingual retrieval.
- R31. Text extraction/OCR: pdfplumber with pytesseract OCR fallback; OCR requires the tesseract binary and must degrade gracefully when absent.
- R32. Vector search: Python/NumPy cosine similarity (app/rag/retriever.py) as the pgvector fallback, isolated behind one module for an easy swap.
- R33. Background jobs: Celery with Redis broker/backend, a 2-second enqueue attempt, and inline fallback when no broker is reachable.
- R34. ML: scikit-learn and pandas remain dependencies only, for a future trained personalization model (nothing running today).
- R35. Testing: pytest, pytest-asyncio, httpx — scaffolded and must be populated.
- R36. Containerization: docker-compose.yml + Dockerfile exist but are unused for local dev.
- R37. Architectural rule (non-negotiable): no router or template may call app/ai/providers/* or app/rag/retriever.py directly; everything routes through llm_gateway.py and rag_service.py so a widget's model and a space's retrieval settings can change from the admin panel with zero code changes.
- R38. Planned infrastructure for the full feature set: transactional email (Postmark or SendGrid); Web Push via VAPID (opt-in, browser-native); full-text search (Postgres tsvector/pg_trgm, Meilisearch if latency demands) scoped to what the requester can already see and never bypassing access control; payments via Stripe or a regional processor if Stripe is unavailable in-market; calendar export via icalendar (.ics) and Google Calendar API; feature flags via .env (Tier-1) + a feature_flags DB table (Tier-2); analytics events in category-separated stores, not a single firehose table.

### 4.4 Roles and access (SRC-D1-§2.4)

- R39. Student: may practice inside the spaces/widgets their groups grant access to, and receives AI tutoring, homework help, and self-graded exercises grounded in real course material.
- R40. Parent: has a read-only view of linked children's readiness score, per-topic mastery, cohort comparison, and activity, and may lock/unlock the child's UI language.
- R41. Teacher/Admin: configures spaces, widgets (incl. LLM connection), corpus, syllabus topics, groups, users, and parent↔child links.

### 4.5 Design system (SRC-D1-§2.5)

- R42. Identity: a "focused study-room" identity — deep ink/navy dark mode for late-night study, with dark mode the implied default.
- R43. Accent discipline: the warm gold accent is reserved only for progress/success moments (streaks, mastery gains, the readiness ring).
- R44. Copy: calm, direct copy with no exclamation marks in system text.
- R45. Typography: a geometric display face for numbers paired with an Arabic-supporting body face (Cairo / IBM Plex Sans Arabic).
- R46. Localization direction: full RTL/LTR support driven by dir="rtl"|"ltr" on <html> from the active locale — not optional, not retrofitted.
- R47. Color tokens (light / dark): --color-bg #e2e8e2/#12261f; --color-surface #ffffff/#0a4530; --color-surface-raised #ffffff/#0f5b3e; --color-teal #0f6b52/#0f6b52; --color-text #12261f/#ffffff; --color-muted #5a6b62/#93a69c; --color-gold #966f1f/#b8912f; --color-gold-light #b8912f/#d9b968; --color-success #0f6b52/#3fae86.

### 4.6 Local setup (SRC-D1-§2.6)

- R48. Local setup must: run PostgreSQL with pgcrypto (pgvector not required); optionally run Redis via docker run; copy .env.example → .env with DATABASE_URL/REDIS_URL; pip install -e ".[dev]"; python -m alembic upgrade head; run the idempotent seed (python -m app.db.seed) creating admin/student/parent accounts, a "Math" space with all widget types, syllabus topics, and seed-PDF RAG ingestion while writing a gitignored creds.txt; then run python app.py at http://127.0.0.1:8000 with reload on.

### 4.7 Known gaps to close (SRC-D1-§2.7)

- R49. Session handling must move from a single 12-hour cookie to real access+refresh (the refresh code exists but is dead/unused).
- R50. Rate limiting must be added for login, chat, and exercise generation (currently unlimited).
- R51. A server-level feature-flag kill switch must exist ahead of the admin panel.
- R52. Usage-time tracking must extend beyond discrete timestamped events.
- R53. An analytics/event pipeline must replace on-read computation from transactional tables.
- R54. Billing/monetization primitives must be added (balance, transaction, paywall).
- R55. The parent-facing cohort-average comparison must enforce a minimum-n guard (ANALYTICS_COHORT_MIN_N) so small cohorts cannot be de-anonymized by inference.

## 5. Contradictions

1. **AI provider architecture.** claude.md specified a pluggable AI provider registry (mock/heuristic/sklearn/llm); §2.2 describes only httpx to an OpenAI-compatible local server via ai/providers/local_llm_provider.py. Resolution (§2.1.2): the 2026-08-09 pivot superseded the registry; the AI-gateway seam is the surviving invariant. Documented supersession, not an open conflict.
2. **Containerization.** claude.md specified a full Docker Compose stack; §2.2 lists docker-compose.yml + Dockerfile as "unused for local dev"; map.md scoped to no-Docker. Resolution: local dev is intentionally non-Docker; container artifacts remain in the repo unused.
3. **Dark vs light default.** §2.5 states dark mode is "the implied default" while the token table defines both full light and dark palettes. Not a conflict; dark is the default theme with a light alternative.
4. **Content hierarchy supersession.** claude.md's subjects→topics→exercises content hierarchy and population-level common-mistake mining became "historical/aspirational" (§2.1.2); the syllabus topic taxonomy and per-topic mastery survive in §2.1.

## 6. Gaps and questions

Missing information an enterprise delivery needs, each mapped to a question:

- **G1 — Production deployment target.** D1 documents only local development; no production topology, hosting, scaling, or decision on whether the LLM stays self-hosted. → Q-001
- **G2 — Scale and volumes.** No user counts, concurrency, corpus size, or request rates. → Q-002
- **G3 — Data-protection jurisdiction.** D1 says "GDPR-style" but Algeria is outside the EU; the applicable local law (e.g. Law 18-07) is not stated. → Q-003
- **G4 — Monetization model.** Billing is a stated gap and pricing/subscription/referral is planned, but who pays, the pricing model, and any free tier are unspecified. → Q-004
- **G5 — AI output language policy.** "Bilingual (Arabic/English, French content)" does not state which language(s) AI answers, exercises, and citations must use. → Q-005
