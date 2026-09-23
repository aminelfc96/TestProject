# Glossary — BAC/BEM Exam Prep Platform

**Purpose:** Defines every domain term and acronym used in source document D1 the way D1 uses it, so the team shares one vocabulary. Entries marked "(inferred)" rely on standard usage because D1 does not spell out the expansion or definition.

| Term | Definition (as used in D1) | Source | Note |
|---|---|---|---|
| BAC | The Algerian national high-school final exam taken by Terminale students. | SRC-D1-§2.1 | Expansion "Baccalauréat" inferred |
| BEM | The Algerian national middle-school final exam taken by 4ème moyenne students. | SRC-D1-§2.1 | Expansion "Brevet d'Enseignement Moyen" inferred |
| Terminale | Last year of high school; the BAC cohort. | SRC-D1-§2.1 | — |
| 4ème moyenne | Final year of middle school; the BEM cohort. | SRC-D1-§2.1 | — |
| Cohort | The set of students whose aggregate metrics are compared (e.g. cohort-average readiness/mastery). | SRC-D1-§2.1, §2.7 | Inferred from usage |
| Space | A subject-scoped area (e.g. "Math") in which a student works, built from reusable widgets. | SRC-D1-§2.1 | — |
| Widget | A reusable functional unit inside a space; the catalog has four types. | SRC-D1-§2.1 | — |
| Chatbot (widget) | General AI tutor chat. | SRC-D1-§2.1 | — |
| Assisting (widget) | Homework-help chat; same chat mechanics as Chatbot but a different system prompt. | SRC-D1-§2.1 | — |
| Exercising (widget) | AI-generated practice problems with a self-graded reveal-solution step. | SRC-D1-§2.1 | — |
| Learning (widget) | A status list of the space's course-material (PDF) library; PDFs are never served raw. | SRC-D1-§2.1 | — |
| RAG (Retrieval-Augmented Generation) | Generation grounded by retrieving relevant passages from a knowledge base so responses cite syllabus content instead of generic model output. | SRC-D1-§2.1 | — |
| Knowledge base / corpus | The set of admin-uploaded course PDFs, ingested into text→chunks→embeddings for retrieval. | SRC-D1-§2.1, §2.3 | — |
| Mastery estimate | A per-topic proficiency estimate for a student, updated by every self-rated exercise attempt. | SRC-D1-§2.1 | — |
| Personalization engine | The subsystem that uses mastery estimates (not randomness) to choose the next topic and difficulty. | SRC-D1-§2.1 | — |
| Readiness score | The BAC/BEM-readiness score rolled up from a student's per-topic mastery estimates. | SRC-D1-§2.1 | — |
| Needs-attention flag | A flag shown to parents on a subject/topic requiring attention. | SRC-D1-§2.1 | — |
| Cohort-average comparison | An anonymized comparison of a student's metrics against the cohort average; requires a minimum-n guard. | SRC-D1-§2.1, §2.7 | — |
| Parent↔child link | The linkage between a parent account and the child (student) account(s) it tracks. | SRC-D1-§2.1, §2.4 | — |
| Admin/teacher panel | The configuration surface for spaces, widgets, corpus, taxonomy, groups, users, and parent links, without code changes. | SRC-D1-§2.1 | — |
| Syllabus topic taxonomy | The structured list of syllabus topics used for mastery, exercises, and RAG scoping. | SRC-D1-§2.1 | — |
| AI-gateway seam | The architectural rule that all LLM calls go through app/services/llm_gateway.py (originally app/ai/registry.py); nothing else calls a provider directly. | SRC-D1-§2.1.2, §2.3 | — |
| LLM (Large Language Model) | The model backing chat and exercise generation. | SRC-D1-§2.2 | — |
| Local LLM server | An OpenAI-compatible local server (Ollama, LM Studio, LocalAI) reached over httpx. | SRC-D1-§2.2 | — |
| Embeddings | Vector representations of text chunks used for retrieval. | SRC-D1-§2.2 | — |
| Cross-lingual retrieval | Retrieval that works across Arabic, French, and English; requires a multilingual embedding model. | SRC-D1-§2.2 | — |
| Cosine similarity | The vector-similarity measure used by the Python/NumPy retriever as the pgvector fallback. | SRC-D1-§2.2 | — |
| pgvector | PostgreSQL vector extension; not installed on the dev machine. | SRC-D1-§2.2 | — |
| pgcrypto | PostgreSQL cryptographic extension; installed. | SRC-D1-§2.2 | — |
| OCR (Optical Character Recognition) | Text extraction from scanned PDFs via pytesseract; requires the tesseract binary and degrades gracefully when absent. | SRC-D1-§2.2 | — |
| JWT (JSON Web Token) | The hand-rolled auth token, delivered in an httponly session cookie (python-jose + passlib[bcrypt]). | SRC-D1-§2.2 | — |
| Celery | The background-job system; broker/backend is Redis. | SRC-D1-§2.2 | — |
| Redis | The Celery broker/backend and an optional local dependency. | SRC-D1-§2.2 | — |
| HTMX | Frontend library used for partial page swaps; no SPA build step. | SRC-D1-§2.2 | — |
| Alpine.js | Lightweight client-side JavaScript framework. | SRC-D1-§2.2 | — |
| RTL / LTR | Right-to-left / left-to-right text direction, driven by the dir attribute on <html> from the active locale. | SRC-D1-§2.5 | — |
| VAPID | Protocol for opt-in, browser-native Web Push. | SRC-D1-§2.2.1 | Expansion inferred |
| GDPR | EU General Data Protection Regulation; D1 references a "GDPR-style" download/delete flow. | SRC-D1-§2.1.1 | Expansion inferred |
| 2FA | Two-factor authentication, part of the profile security tab. | SRC-D1-§2.1.1 | — |
| XP | Experience points in the gamification system. | SRC-D1-§2.1.1 | — |
| Leaderboard | An opt-in ranking scoped to a student's own groups, never a raw global ranking. | SRC-D1-§2.1.1 | — |
| Feature flags | Kill switches for rolling features on/off: .env (Tier-1) plus a feature_flags DB table (Tier-2). | SRC-D1-§2.2.1, §2.7 | — |
| tsvector / pg_trgm | PostgreSQL full-text search primitives (initial search backend). | SRC-D1-§2.2.1 | — |
| Meilisearch | Search engine fallback if Postgres full-text search latency is insufficient. | SRC-D1-§2.2.1 | — |
| Stripe | Payment processor; a regional alternative is acceptable if Stripe is unavailable in-market. | SRC-D1-§2.2.1 | — |
| iCal (.ics) | Calendar export format produced via the icalendar Python library. | SRC-D1-§2.2.1 | — |
| Analytics/event pipeline | A planned store for product events, category-separated rather than a single firehose table. | SRC-D1-§2.2.1, §2.7 | — |
| ANALYTICS_COHORT_MIN_N | plan.md setting enforcing a minimum cohort size for the parent cohort-average comparison. | SRC-D1-§2.7 | — |
| Inline fallback | Celery's behavior of executing a task in-process when no broker is reachable. | SRC-D1-§2.2 | — |
