# Implementation Plan: Vodafone Cash Payment

**Branch**: `002-vodafone-cash-payment` | **Date**: 2026-04-10 | **Spec**: [specs/002-vodafone-cash-payment/spec.md](specs/002-vodafone-cash-payment/spec.md)
**Input**: search on the internet for any information u need and use daisy ui and redux tool kit if needed and implement unit test and use feature slice design pattern

## Summary

Implement a manual Vodafone Cash payment flow for courses. Teachers can configure a default 010-prefixed Vodafone Cash number, students view payment instructions and upload a transaction receipt screenshot, and teachers get a dashboard to approve or deny the manual payment. Implemented using React, TailwindCSS + DaisyUI, Redux Toolkit, and adhering to Feature-Sliced Design (FSD).

## Technical Context

**Language/Version**: TypeScript 5.9+, React 19  
**Primary Dependencies**: 
- **State & Data**: `@reduxjs/toolkit`, `@tanstack/react-query`
- **UI & Styling**: `tailwindcss`, `daisyui`, `motion` (for animations)
- **Forms**: `react-hook-form`, `@hookform/resolvers`, `zod`
**Storage**: `appwrite` (Database collections and Storage bucket for receipts)  
**Testing**: `vitest`, `@testing-library/react`
**Target Platform**: Web application  
**Project Type**: Frontend web application  
**Performance Goals**: < 2s load time (React Query / RTK Query caching)  
**Constraints**: FSD architecture (api, components, hooks, pages, tests), strict 010 validation  
**Scale/Scope**: Up to 1,000 enrolled students per course  

## Constitution Check

*GATE: Passed*
- **I. Master Plan Adherence**: React 19, Vite, TS, TailwindCSS v4, Appwrite, Redux Toolkit, Zod.
- **II. Feature-Sliced Design (FSD)**: Implementing features into `features/payments` and `features/enrollments`.
- **IV. Exact Data Model Consistency (ERD)**: Payment models aligned with constitution/ERD boundary. 
- **V. Product Scope & Quality Gates**: Implement unit tests, validation, and manual verification flow without merging logic incorrectly.
- **VI. Performance & Security Best Practices**: Strict Zod validation on inputs (11-digit `010` numbers) and restrictive Appwrite permissions for receipt images.

## Project Structure

### Documentation (this feature)

```text
specs/002-vodafone-cash-payment/
├── plan.md              
├── research.md          
├── data-model.md        
├── quickstart.md        
└── contracts/           
```

### Source Code (repository root)

```text
src/
├── app/
│   └── store.ts                  # Integrates RTK slices
├── entities/
│   ├── payment/                  # Payment RTK slices and Zod schemas
│   └── user/
├── features/
│   ├── teacher/                  # Teacher default number setup & approval logic
│   │   ├── api/                  # React Query hooks for Appwrite DB updates
│   │   ├── components/           # DaisyUI + Motion components (Approval tables, Settings form)
│   │   ├── hooks/                # Local logic (useTeacherPayment)
│   │   ├── pages/                # PendingEnrollmentsPage
│   │   ├── tests/                # Vitest unit tests for components and hooks
│   │   └── index.ts              # Public API for this feature
│   ├── student/                  # Student payment submission
│   │   ├── api/                  # React Query mutations for Appwrite DB / Storage upload
│   │   ├── components/           # DaisyUI forms (react-hook-form + zod) & Motion modals
│   │   ├── hooks/                # useSubmitPayment
│   │   ├── pages/                # StudentPaymentPage
│   │   ├── tests/                # Vitest tests
│   │   └── index.ts              # Public API for this feature
│   ├── courses/                  # Course-level Vodafone number setup
│   │   ├── api/                  
│   │   ├── components/           # Course overrides DaisyUI forms
│   │   ├── hooks/                
│   │   ├── pages/                
│   │   ├── tests/                
│   │   └── index.ts              
├── shared/
│   ├── ui/                       # Reusable purely visual DaisyUI wrapped blocks
│   └── test/                     # Test utilities
```

**Structure Decision**: The frontend heavily leverages Feature-Sliced Design (FSD). Inside every feature slice (`teacher`, `student`, `courses`), code is strictly separated into `api` (React Query), `components` (DaisyUI, react-hook-form, motion), `hooks`, `pages`, and `tests` (Vitest). Data fetching flows through `@tanstack/react-query`, global client state (like active UI states) flows through `@reduxjs/toolkit`, and validation goes through `zod`.
