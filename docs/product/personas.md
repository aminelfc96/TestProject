# Personas — BAC/BEM Exam Prep Platform

**Purpose:** The four user roles the product serves, each evidenced by source document D1. Attributes D1 does not state are marked "inferred" and recorded as assumptions (ASM-011–ASM-013).

The four personas below correspond one-to-one to the roles D1 defines (SRC-D1-§2.4). D1 describes only these roles; no additional personas are introduced.

## 1. BAC student (Terminale)

- **Who:** A last-year high-school student preparing for the BAC national exam (SRC-D1-§2.1, §2.4). D1 characterizes the audience as teenage and under exam pressure (SRC-D1-§2.1.2). Typical age ~17–18 *(inferred — ASM-011)*.
- **Goals:** Prepare for the BAC; practice in the subject spaces their groups grant; get AI tutoring, homework help and self-graded exercises grounded in real course material (SRC-D1-§2.1, §2.4).
- **Pains:** Exam pressure and late-night study (SRC-D1-§2.1.2, §2.5); tutoring that may not target the actual national syllabus *(inferred from the grounding requirement — ASM-012)*.
- **Context of use:** Works inside subject-scoped spaces of widgets; the interface is calm with dark mode as the implied default for late-night study (SRC-D1-§2.1, §2.5).
- **How the product helps:** Grounded, cited AI tutoring; self-graded practice that updates per-topic mastery; a next-topic/difficulty driven by mastery; a readiness score (SRC-D1-§2.1).

## 2. BEM student (4ème moyenne)

- **Who:** A final-year middle-school student preparing for the BEM national exam (SRC-D1-§2.1, §2.4). Typical age ~14–15 *(inferred — ASM-011)*.
- **Goals:** Same as the BAC student, for the BEM cohort: practice in granted spaces and get grounded tutoring, homework help and self-graded exercises (SRC-D1-§2.1, §2.4).
- **Pains:** Exam pressure; tutoring not aligned to the syllabus *(inferred — ASM-012)*.
- **Context of use:** The same spaces + widgets learning loop as the BAC cohort; D1 does not describe cohort-specific features beyond the two exam types (SRC-D1-§2.1).
- **How the product helps:** The same learning loop: grounded tutoring, mastery personalization, readiness score (SRC-D1-§2.1).

## 3. Parent

- **Who:** A read-only observer of one or more linked child accounts (SRC-D1-§2.4).
- **Goals:** Track the child's readiness score, per-topic mastery, cohort comparison and activity; spot the "needs attention" flag; lock or unlock the child's UI language (SRC-D1-§2.1, §2.4).
- **Pains:** Paying for private tutoring without visibility into actual exam readiness *(inferred from the inception brief's market context — ASM-012)*; no way to know where the child needs help *(inferred — ASM-012)*.
- **Context of use:** Read-only dashboard; the parent does not practice, only observes and can set the child's UI language (SRC-D1-§2.4).
- **How the product helps:** Per-subject/per-topic readiness, a needs-attention flag and an anonymized cohort comparison replace blind payment with visible progress (SRC-D1-§2.1).

## 4. Teacher / Admin

- **Who:** The operator who runs the platform for a cohort or school (SRC-D1-§2.4).
- **Goals:** Configure spaces, the widget catalog (including which local LLM backs each widget), the document corpus, the syllabus topic taxonomy, groups, users and parent↔child links — without code changes (SRC-D1-§2.1, §2.4).
- **Pains:** Needing engineering to change what the platform offers — the "without code changes" requirement implies code change is the current alternative (SRC-D1-§2.1).
- **Context of use:** The admin/teacher panel (SRC-D1-§2.1).
- **How the product helps:** A single no-code configuration surface for all of the above (SRC-D1-§2.1).

## Inferred attributes (assumptions)

| Assumption | Statement | Why it matters | If wrong |
|---|---|---|---|
| ASM-011 | BAC candidates are ~17–18 and BEM candidates ~14–15 years old. | Sets tone and reading level for a "teenage, exam-pressure" audience. | Persona wording may be slightly off; product behavior is unaffected. |
| ASM-012 | The parent's core pain is paying for tutoring without visible exam readiness, and students' pain is tutoring not aligned to the syllabus. | Anchors the parent and student value propositions. | The value framing may miss a different dominant motivation. |
| ASM-013 | "Groups" are the access-control unit linking students to spaces/widgets. | Shapes how we frame access, cohorts and leaderboard scoping. | Access/cohort framing may need revision when Part II is supplied. |
