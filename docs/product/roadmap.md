# Roadmap — BAC/BEM Exam Prep Platform

**Purpose:** Forward-looking phases expressed as outcomes, MVP first, with the evidence and ordering rationale for each. No dates — D1 provides none.

## How to read this

D1 describes a working core (report.md) with known gaps, a next implementation pass (plan.md), and a full feature set (EPIC-15→24 in Part II, not supplied). This roadmap sequences those into outcome-based phases. Phase groupings of the EPIC-15→24 surface are provisional until Part II is supplied (ASM-015).

## Phase 1 — Safe, trustworthy core (MVP)

**Outcome:** A BAC/BEM student practices in subject-scoped spaces with AI tutoring and self-graded exercises that are grounded in and cite real course PDFs; a linked parent sees a trustworthy readiness view (per-topic mastery, needs-attention flag, anonymized cohort comparison); admins configure everything without code — and it is safe to put in front of real users.

**In scope:** the core learning loop (SRC-D1-§2.1) plus the gaps that make it unsafe or unmeasurable: access+refresh sessions, rate limiting, a feature-flag kill switch, the cohort minimum-n guard, usage-time tracking and the analytics pipeline (SRC-D1-§2.7); legal/compliance (consent, Terms of Service, Privacy Policy, GDPR-style download/delete) as a precondition for real users — exact scope depends on Q-003 (SRC-D1-§2.1.1).

**Why first:** the current core cannot be responsibly operated at real-user scale until the session, rate-limit, kill-switch and cohort-privacy gaps are closed, and outcomes cannot be measured until analytics and usage tracking exist (SRC-D1-§2.7). Billing primitives are gated on the unresolved monetization model (Q-004).

## Phase 2 — Guided start and sustained engagement

**Outcome:** New students start in minutes and stay in a consistent study habit; parents stay informed of progress and at-risk moments.

**In scope:** onboarding wizard, notifications center, gamification (XP, badges, opt-in group leaderboard), and study planner & calendar export (SRC-D1-§2.1.1).

**Why second:** these build on a safe core and drive the engagement and retention outcomes (M3, M5) defined in success-metrics.md.

## Phase 3 — Support, community and self-service trust

**Outcome:** Students get help beyond the AI and trust the product and their own data.

**In scope:** help center, support chat & feedback, global search, profile & security incl. 2FA, community/study groups, and changelog (SRC-D1-§2.1.1).

**Why third:** these deepen retention and trust once the core and engagement loops are proven.

## Phase 4 — Commercial scale

**Outcome:** The platform can charge for its value and measure business outcomes.

**In scope:** pricing, subscription & referral plus billing primitives — balance, transaction, paywall (SRC-D1-§2.1.1, §2.7).

**Why last:** monetization depends on the unresolved model (Q-004) and only matters once the value (Phases 1–3) exists to be paid for.

## Deferred / not in this roadmap

| Item | Reason | Source |
|---|---|---|
| Production deployment topology and scale | D1 documents only local development; no production target (Q-001, Q-002). | SRC-D1-§2.2, §2.6 |
| Trained/custom ML personalization model | scikit-learn/pandas are dependencies only; nothing runs today. | SRC-D1-§2.2 |
| Subjects→topics→exercises content hierarchy and population-level common-mistake mining | Superseded by the spaces + widgets direction. | SRC-D1-§2.1.2 |
| Raw global leaderboards | Excluded by design: leaderboards are opt-in and group-scoped. | SRC-D1-§2.1.1 |
