---
description: "Task list for Vodafone Cash Payment feature"
---

# Tasks: Vodafone Cash Payment

**Input**: Design documents from `/specs/002-vodafone-cash-payment/`
**Prerequisites**: plan.md, spec.md, research.md, data-model.md
**Organization**: Tasks are grouped by user story. FSD rules are enforced securely. Types, schemas, store slices, and tests are explicitly detailed.

## Phase 1: Setup & Foundational (Shared Entities & Types)

**Purpose**: Core Zod schemas and global Redux mappings that block all user stories.

- [ ] T001 Define `VodafoneNumberSchema` using Zod inside `src/entities/payment/schemas/vodafoneNumber.ts`
- [ ] T002 [P] Export typescript definition `PaymentMethod` and `PaymentStatus` in `src/entities/payment/types/index.ts`
- [ ] T003 Setup Redux Toolkit structured slices by creating a `store` folder inside each feature (`src/features/teacher/store/`, `src/features/student/store/`) to hold their respective slices, and import them into `src/app/store.ts`.
- [ ] T004 Create shared `FileUploadBtn` component leveraging DaisyUI inside `src/shared/ui/FileUploadBtn.tsx`

---

## Phase 2: User Story 1 - Teacher Configures Default Vodafone Number (P1)

**Goal**: Teacher can set and save their default 11-digit Vodafone Cash number.

### Tests for User Story 1 
- [ ] T005 [P] [US1] Create unit tests for teacher payment settings hook in `src/features/teacher/tests/useTeacherPayment.test.ts`
- [ ] T006 [P] [US1] Create React Testing Library UI tests in `src/features/teacher/tests/TeacherPaymentForm.spec.tsx`

### Implementation for User Story 1
- [ ] T007a [P] [US1] Create pure Appwrite API methods in `src/features/teacher/api/teacherPaymentApi.ts`
- [ ] T007b [P] [US1] Create `useUpdateTeacherNumber` React Query hook inside `src/features/teacher/hooks/useTeacherPayment.ts`
- [ ] T008 [US1] Create Zod form resolver and Hook Form for TeacherPaymentForm in `src/features/teacher/components/TeacherPaymentForm.tsx` (Depends on T001)
- [ ] T009 [US1] Export components from `src/features/teacher/index.ts`
- [ ] T010 [US1] Mount `TeacherPaymentForm` strictly into the teacher profile settings page at `src/features/teacher/pages/SettingsPage.tsx`

---

## Phase 3: User Story 2 - Course Payment Assignment (P1)

**Goal**: Teacher can override default Vodafone cash number and toggle re-submissions per course.

### Tests for User Story 2
- [ ] T011 [P] [US2] Create unit test for course payment override hook in `src/features/courses/tests/useCoursePayment.test.ts`

### Implementation for User Story 2
- [ ] T012 [P] [US2] Define course payment schemas and types in `src/features/courses/schemas/coursePayment.ts`
- [ ] T013a [P] [US2] Create pure Appwrite API methods for patching course settings in `src/features/courses/api/coursePaymentApi.ts`
- [ ] T013b [P] [US2] Create React Query hook to patch `vodafone_number` and `allow_resubmission` in `src/features/courses/hooks/useCoursePayment.ts`
- [ ] T014 [US2] Build `CoursePaymentStep` DaisyUI component supporting radio selection (default vs custom) in `src/features/courses/components/CoursePaymentStep.tsx`
- [ ] T015 [US2] Integrate `CoursePaymentStep` into the course creation wizard in `src/features/teacher/pages/CreateCoursePage.tsx`

---

## Phase 4: User Story 3 - Student Submits Payment Request (P2)

**Goal**: Student views payment instructions and uploads a receipt screenshot.

### Tests for User Story 3
- [ ] T016 [P] [US3] Create upload and form state tests in `src/features/student/tests/StudentPaymentForm.spec.tsx`

### Implementation for User Story 3
- [ ] T017 [P] [US3] Define Zod schema for receipt upload form in `src/features/student/schemas/submitPayment.ts`
- [ ] T018a [P] [US3] Create pure Appwrite API methods to mutate the Payments collection and upload image to Appwrite Storage in `src/features/student/api/submitPaymentApi.ts`
- [ ] T018b [P] [US3] Create React Query hook executing the submit payment API in `src/features/student/hooks/useSubmitPayment.ts`
- [ ] T019 [US3] Build `StudentPaymentForm` UI using DaisyUI, integrating `FileUploadBtn` in `src/features/student/components/StudentPaymentForm.tsx`
- [ ] T020 [US3] Create new routed page `src/features/student/pages/StudentPaymentPage.tsx` to handle the full view for course payments.

---

## Phase 5: User Story 4 - Teacher Reviews Pending Enrollments (P2)

**Goal**: Teacher can approve or deny student manual payments from a course dashboard.

### Tests for User Story 4
- [ ] T021 [P] [US4] Create tests for enrollment actions in `src/features/teacher/tests/PendingEnrollmentsTable.spec.tsx`

### Implementation for User Story 4
- [ ] T022 [P] [US4] Define `PendingEnrollment` types in `src/features/teacher/types/enrollments.ts`
- [ ] T023a [P] [US4] Create pure Appwrite queries to fetch `Payments` and mutate `payment_status` in `src/features/teacher/api/enrollmentsApi.ts`
- [ ] T023b [P] [US4] Create React Query hooks utilizing the API in `src/features/teacher/hooks/useEnrollmentsAction.ts`
- [ ] T024 [US4] Build `PendingEnrollmentsTable` with DaisyUI and Motion (for row exit animation) in `src/features/teacher/components/PendingEnrollmentsTable.tsx`
- [ ] T025 [US4] Integrate table into `src/features/teacher/pages/CourseEnrollmentsPage.tsx`

---

## Phase 6: User Story 5 - Student Receives Feedback Status (P3)

**Goal**: Students can see their `Pending`, `Approved`, or `Denied` status.

### Implementation for User Story 5
- [ ] T026 [P] [US5] Build isolated `EnrollmentStatusBadge` component using DaisyUI variants in `src/features/student/components/EnrollmentStatusBadge.tsx`
- [ ] T027 [US5] Integrate badge hook onto `src/features/student/pages/StudentDashboardPage.tsx` and the core course view components.

---

## Phase 7: User Story 6 - Teacher Monitors Pending Alerts (P2)

**Goal**: Badge alerting teachers of pending reviews globally and per course.

### Tests for User Story 6
- [ ] T028 [P] [US6] Create Redux state unit tests for pending counts in `src/features/teacher/tests/pendingSlice.test.ts`

### Implementation for User Story 6
- [ ] T029 [P] [US6] Create RTK slice tracking pending enrollment aggregations in `src/features/teacher/store/pendingSlice.ts` and export to root.
- [ ] T030a [P] [US6] Create pure Appwrite API observer methods for pending counts in `src/features/teacher/api/pendingCountsApi.ts`
- [ ] T030b [P] [US6] Create RTK query or React Query observer to update Redux pending counts real-time in `src/features/teacher/hooks/useGlobalPendingCounts.ts`
- [ ] T031 [US6] Build global alert dropdown and course-card badges in `src/features/teacher/components/PendingAlertBadge.tsx`
- [ ] T032 [US6] Mount `PendingAlertBadge` in the main Teacher header located at `src/features/teacher/components/TeacherHeader.tsx`

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Cleanup, security rules confirmation, and UX refinement.
- [ ] T033 Double-check that Appwrite Database constraints strictly limit access based on `reviewed_by` and `payment_method`.
- [ ] T034 Verify DaisyUI theming matches light/dark modes correctly within the payment flows.
- [ ] T035 Ensure RTK Query cache invalidation reliably forces table updates when actions occur without an explicit browser reload.

---

## Dependencies & Execution Order

### Phase Dependencies
- **Phase 1 (Setup)**: Begins immediately.
- **Phases 2 & 3 (Teacher Setup)**: Can run immediately after Setup.
- **Phase 4 (Student Submission)**: Depends broadly on Phase 2 & 3 (needs Vodafone numbers configured).
- **Phases 5 & 7 (Teacher Approval & Alerts)**: Depends on Phase 4 (requires student Submissions).
- **Phase 6 (Student Feedback)**: Depends on Phase 5.

### Parallel Opportunities
- Zod Schemas [P] (T001, T002) can be created freely.
- All Unit test files [P] (T005, T006, T011, T016, T021, T028) can be created concurrently.
- API Queries and Mutations marked [P] can run in parallel while components are actively developed.
