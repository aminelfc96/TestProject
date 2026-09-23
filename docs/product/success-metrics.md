# Success Metrics — BAC/BEM Exam Prep Platform

**Purpose:** The outcome metrics the product will be judged by, each defined with a measurement source. D1 states no baselines or numeric targets; where a target is missing we raise a question instead of inventing a number.

## What "outcome" means here

An outcome metric measures a change for a user or the business (readiness improves, a habit sticks, a parent can see progress). Output metrics — number of exercises generated, PDFs uploaded, chats sent — are deliberately excluded: they measure what we built, not whether it worked.

## Metrics

| ID | Outcome metric | Definition | How measured | Baseline | Target | Source |
|---|---|---|---|---|---|---|
| M1 | Readiness improvement | Increase in a student's BAC/BEM-readiness score over a study period. | The readiness score rolled up from per-topic mastery estimates. | Not stated in D1. | Not stated → Q-012. | SRC-D1-§2.1 |
| M2 | Per-topic mastery gain | Increase in a student's mastery estimate for a topic after self-rated attempts on that topic. | Every self-rated exercise attempt updates the per-topic mastery estimate. | Not stated in D1. | Not stated → Q-012. | SRC-D1-§2.1 |
| M3 | Study consistency | A sustained weekly study habit that supports readiness (streak persistence, topics-due-for-review completion). | Usage-time tracking (currently a gap) plus streak/review signals from notifications and gamification. | Not stated in D1. | Not stated → Q-013. | SRC-D1-§2.1.1, §2.7 |
| M4 | Grounded-answer trust | The share of AI responses students rate helpful versus unhelpful, as a proxy for trust in the grounded answers. | Thumbs-up/down plus comment on every AI response. | Not stated in D1. | Not stated → Q-014. | SRC-D1-§2.1.1 |
| M5 | Parent readiness visibility | The share of linked parents who regularly view their child's readiness/mastery (the visibility actually reaches parents). | Parent activity on the read-only readiness view. | Not stated in D1. | Not stated → Q-015. | SRC-D1-§2.1, §2.4 |
| M6 | Business value | Paid conversion and retention once monetization exists. | Depends on the pricing/subscription/referral model and billing primitives (currently a gap). | Not stated in D1. | Not stated → Q-016 (depends on Q-004). | SRC-D1-§2.1.1, §2.7 |

## Measurement prerequisites

M3–M6 depend on infrastructure that is itself a stated gap and must be built first: usage-time tracking (R52) and an analytics/event pipeline (R53). Until those exist, these metrics cannot be measured at scale (SRC-D1-§2.7).

## Note on baselines and targets

D1 contains no baseline figures and no numeric targets for any of these metrics. Rather than invent numbers, baselines and targets are raised as questions Q-012–Q-016 for the Super Admin to answer.
