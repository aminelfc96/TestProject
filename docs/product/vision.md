# Product Vision — BAC/BEM Exam Prep Platform

**Purpose:** One page stating what we are building, for whom, why, and what is deliberately out of scope, so any stakeholder can restate the direction in one sentence.

## Vision

Give Algerian BAC and BEM students trustworthy, personalized exam preparation — AI tutoring and self-graded practice grounded in real course material, not generic chatbot output — while parents can see how their child is actually progressing and teachers can configure the platform without code. *(SRC-D1-§2.1)*

## Problem statement

Algeria runs two education systems in parallel: a free public one, and a private tutoring economy on top of it that households treat as compulsory for any child approaching the BEM or the BAC, paying per subject per month across four to six subjects for two or three years (Inception brief — Super Admin project description). The product's job is to make exam preparation trustworthy and visible:

- A student needs help that targets the **actual national syllabus**, so AI answers and generated exercises must be grounded in real, admin-uploaded course PDFs and cite syllabus content rather than produce generic model output (SRC-D1-§2.1).
- A parent needs to **see** whether the effort is working — readiness, per-topic mastery, a "needs attention" flag, and an anonymized cohort comparison (SRC-D1-§2.1).
- A teacher/operator needs to **change the product** (spaces, widgets, models, corpus, topics, groups, users, parent links) without code (SRC-D1-§2.1).

## Target users

| User | Role in the product | Source |
|---|---|---|
| Student — BAC candidate (Terminale) | Practices in granted subject spaces; gets grounded AI tutoring, homework help and self-graded exercises. | SRC-D1-§2.1, §2.4 |
| Student — BEM candidate (4ème moyenne) | Same learning loop for the BEM cohort. | SRC-D1-§2.1, §2.4 |
| Parent | Read-only view of linked children's readiness, mastery, cohort comparison and activity; can lock/unlock the child's UI language. | SRC-D1-§2.1, §2.4 |
| Teacher / Admin | Configures spaces, widgets (incl. which LLM backs each), corpus, syllabus topics, groups, users and parent↔child links. | SRC-D1-§2.1, §2.4 |

## Value proposition

- **For students:** tutoring and practice that can cite the actual syllabus, personalized to demonstrated mastery rather than random (SRC-D1-§2.1).
- **For parents:** visible, comparable proof of exam readiness with a needs-attention flag (SRC-D1-§2.1) — replacing blind payment for tutoring *(inferred from the inception brief's market context — ASM-012)*.
- **For teachers/admins:** a platform they can run and adapt without engineering (SRC-D1-§2.1).

## Product boundaries

### In scope

- Bilingual (Arabic/English) with French content, full RTL/LTR (SRC-D1-§2.1, §2.5).
- Two cohorts: BAC (Terminale) and BEM (4ème moyenne) (SRC-D1-§2.1).
- Subject-scoped spaces built from four widgets: Chatbot, Assisting, Exercising, Learning (SRC-D1-§2.1).
- RAG-grounded chat and exercises that cite real syllabus content (SRC-D1-§2.1).
- Personalization: self-rated attempts update per-topic mastery; mastery (not randomness) drives the next topic and difficulty; mastery rolls up into a readiness score (SRC-D1-§2.1).
- Parent analytics: per-subject/per-topic readiness, needs-attention flag, anonymized cohort comparison (SRC-D1-§2.1).
- No-code admin/teacher panel (SRC-D1-§2.1).
- The standard SaaS surface: onboarding, notifications, gamification, global search, help/support/feedback, profile & security, pricing/subscription/referral, legal/compliance, study planner & calendar, community groups, changelog (SRC-D1-§2.1.1).

### Explicitly out of scope

- Serving course PDFs to students as raw downloadable files (SRC-D1-§2.1).
- A generic, ungrounded chatbot — answers must cite syllabus content (SRC-D1-§2.1).
- Raw global leaderboards — gamification is opt-in and scoped to the student's own groups (SRC-D1-§2.1.1).
- The superseded subjects→topics→exercises content hierarchy and population-level "common mistake" mining (SRC-D1-§2.1.2).
- A trained/custom ML personalization model — scikit-learn/pandas are dependencies only; nothing runs today (SRC-D1-§2.2).
- A single-page-app frontend build — the product is Jinja2 + HTMX + Alpine, no SPA (SRC-D1-§2.2).
- Curriculum authoring — the platform ingests admin-uploaded course PDFs (SRC-D1-§2.1); authoring the curriculum is not a product function *(inferred — ASM-022)*.
- Production deployment topology and scale — D1 documents only local development (see Q-001, Q-002).
- A specific monetization model (who pays, pricing, free tier) — unspecified in D1 (see Q-004).

## Guiding principles for trade-offs

1. **Trust over breadth.** An AI answer that cannot cite real syllabus content is worse than a narrower answer that can (SRC-D1-§2.1).
2. **Personalize from real data, not randomness.** The next topic and difficulty follow demonstrated mastery, never a random pick (SRC-D1-§2.1).
3. **Configure without code.** Everything a teacher/operator may change must be reachable from the panel; the AI-gateway seam keeps model/provider changes out of code (SRC-D1-§2.1, §2.3).
4. **Calm and competent, for an exam-pressure, teenage audience.** Dark mode is the implied default; gold is reserved for progress; no exclamation marks in system copy (SRC-D1-§2.1.2, §2.5).
5. **Privacy and safety by design.** Anonymized comparisons with a minimum-n guard, no raw rankings, and GDPR-style download/delete rights (SRC-D1-§2.1.1, §2.7).
6. **Close safety gaps before scaling.** Sessions, rate limiting, a kill switch and usage analytics are prerequisites to real users, not afterthoughts (SRC-D1-§2.7).
