<!--
Sync Impact Report:
Version change: 1.3.0 -> 1.4.0
Modified principles:
- Updated Principle II. Feature-Sliced Design (FSD) to enforce strict naming separation between pure APIs, React Query hooks, and local stores.
Templates requiring updates: (✅ updated / ⚠ pending) 
- .specify/templates/plan-template.md (⚠ pending - structure update needed)
- .specify/templates/tasks-template.md (⚠ pending - strict task naming needed)
-->

# EduStream Constitution

## Core Principles

### I. Master Plan Adherence
The project strictly follows the architecture, stack, and phases laid out in `context/implementation_plan.md`. This includes maintaining React 19, Vite, TypeScript, TailwindCSS v4, Appwrite, Stripe, React Query, Redux Toolkit, Zod, and Motion. All updates must sync with the master plan.

### II. Feature-Sliced Design (FSD) Strict Architecture
The structure of the project must follow the Features design pattern / slices design pattern (FSD). Code should be strictly organized into domain-specific slices (e.g., `features/auth`, `features/courses`), minimizing coupling between features. Global shared code goes into shared layers (`src/shared/ui`, `src/services/`, etc.). 

**Mandatory Internal Feature Structure & Naming**:
- **`api/`**: Contains pure Appwrite data fetching methods. File names MUST NOT start with `use`. They should end with `Api.ts` (e.g., `submitPaymentApi.ts`).
- **`hooks/`**: Contains pure React Query implementations. File names MUST start with `use` (e.g. `useSubmitPayment.ts`).
- **`store/`**: Contains the Redux Toolkit slice associated exclusively with the feature (e.g. `pendingSlice.ts`).
- **`pages/`**: Contains the routed page layouts for the feature. Pages belonging to a specific feature (like Teacher actions) must live exclusively in `src/features/[featureName]/pages/`.
- **`components/`** and **`tests/`** must be co-located within the feature.

### III. Incremental Phase Execution
Implementation must progress exactly according to the active phases defined in `context/implementation_plan.md`. Currently, **Phase 5 is deliberately skipped**, and development is strictly focused on **Phase 6 (Student Features: Reviews, Notifications)**. No features from other phases should be implemented without explicit amendment.

### IV. Exact Data Model Consistency (ERD)
The database definitions and Appwrite Collections must perfectly match `context/ERD.md`. Crucial architectural choices such as independent Subscription and Payment collections must not be merged. Entities must respect the `Section -> Lesson` hierarchy and all relations. Any enhancements should prioritize adding columns to existing collections rather than spawning disjoint side-collections.

### V. Product Scope & Quality Gates (PRD)
Features must trace back to the requirements in `context/PRD.md`. Development must respect Non-Functional Requirements including: < 2s load time targets, JWT authentication, precise Role-Based Access Control (Student, Teacher, Admin), and Content Protection techniques (device limits, basic visual watermarking).

### VI. Performance & Security Best Practices
- **Security Checkpoints**: Ensure strict Zod validation on inputs on both client and database functions. Apply correct Appwrite collection limits. Tokens, device tracking logic, and Appwrite document permissions must rigorously restrict cross-tenant views.
- **Performance Thresholds**: Code must be audited to ensure compliance with the < 2s load time. Lazy load non-critical React chunks. Leverage React Query’s built-in caching where data is mostly static. 

### VII. Human-in-the-Loop Constraint 
The AI must strictly await explicit human approval before executing any destructive operations. No major Git branch changes (like push/merge), large architectural rewrites, or database schema mutations should be fully executed automatically without prompting for the user's manual "Continue" or "Approve" statement. AI acts as an advisory implementer but the Human steers.

## Governance

- **Amendment Procedure:** Amendments require documentation and updating this constitution file to reflect new phases or design decisions. All PRs must verify compliance with PRD and ERD boundaries.
- **Versioning Policy:** Major version bumps for architectural shifts, minor for new phases or principles, patch for clarifications.
- **Compliance Review Expectations:** All pull requests, tasks, and feature specifications must verify compliance with the `context/implementation_plan.md`, Phase 6 goals, `context/PRD.md`, and `context/ERD.md`. Complexity that breaks FSD must be strictly justified in architecture documents.

**Version**: 1.4.0 | **Ratified**: 2026-04-09 | **Last Amended**: 2026-04-10
