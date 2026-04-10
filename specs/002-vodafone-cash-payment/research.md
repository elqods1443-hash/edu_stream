# Research: Vodafone Cash Payment Feature

## Phase 0: Outline & Technical Decisions

### 1. UI Framework: DaisyUI + TailwindCSS v4
- **Decision**: Use DaisyUI on top of TailwindCSS v4 for forms, modals, tables, and the layout.
- **Rationale**: The user explicitly instructed to "use daisy ui... if needed". DaisyUI's pre-built UI components (like `file-input`, `table`, `btn`, `alert`, and `badge`) make constructing the complex forms, the teacher upload table, and the payment status badges extremely fast, reliable, and visually consistent without writing extensive custom CSS.
- **Alternatives considered**: Raw Tailwind components (Headless UI), but DaisyUI provides the required visual polish faster. FSD FSD atomic components will wrap DaisyUI classes.

### 2. State Management: Redux Toolkit (RTK) & RTK Query
- **Decision**: Integrate Redux Toolkit for complex UI states (e.g. tracking pending approval counts across the entire dashboard) and manage data fetching where required.
- **Rationale**: User explicitly instructed to "use redux tool kit". Redux Toolkit is well suited for the Feature-Sliced Design pattern by isolating state into feature-specific slices (e.g. `pendingEnrollmentsSlice`).
- **Alternatives considered**: React Context or Zustand, but RTK is requested and scales well for large enterprise patterns.

### 3. Unit Testing Strategy
- **Decision**: Implement tests with Vitest and React Testing Library using FSD structure (co-locating `spec.tsx` and `test.ts` within the slices).
- **Rationale**: Specified in constraints ("implement unit test"). Given Vite is in use, Vitest maps perfectly to Vite's build config. 

### 4. Validation Rules for Vodafone Egypt
- **Decision**: Use Zod to enforce Vodafone Cash validations. The regex `/^010\d{8}$/` will validate that numbers have exactly 11 digits and start with `010`.
- **Rationale**: Vodafone Egypt exclusively uses the `010` prefix. Rejecting `011`, `012`, `015` immediately prevents failed offline transfers.

### 5. File Uploads (Transaction Receipts)
- **Decision**: Receipts will be uploaded to an Appwrite Storage bucket securely, generating a read-only URL to link to the Enrollment document.
- **Rationale**: Manual verification requires the teacher to visually read the receipt. Appwrite Storage handles limits and file types (e.g. `image/jpeg`, `image/png`) gracefully.
