# Data Model: Vodafone Cash Payment Feature

Here are the Appwrite collection schema updates for the manual Vodafone Cash payment system. Instead of creating new collections, this feature extends the existing `Users`, `Courses`, and `Payments` collections defined in `context/ERD.md` to cleanly integrate manual verification.

## 1. `Users` Collection (Extended)
Extending the existing Users collection to support the teacher's default payment settings.

| Field | Type | Required | Validation | Description |
|-------|------|----------|------------|-------------|
| `default_vodafone_number` | String | No | Regex: `^010\d{8}$` | Only used if `role` == 'teacher'. The 11-digit mobile number starting with 010. |

## 2. `Courses` Collection (Extended)
Extending the existing Courses collection to support course-specific payment instructions.

| Field | Type | Required | Validation | Description |
|-------|------|----------|------------|-------------|
| `vodafone_number` | String | No | Regex: `^010\d{8}$` | Overrides the teacher's default number for this specific course. Only used if `price` > 0. |
| `allow_resubmission` | Boolean | No | default: `false` | If true, denied students can re-submit a payment attempt for this course. |

## 3. `Payments` Collection (Extended)
Extending the existing Payments tracking collection to handle manual Vodafone Cash enrollments without creating a new `Enrollment` table. The existing `payment_status` field inherently handles `Pending`, `Approved` (Mapped to `success`), and `Denied` (Mapped to `failed`).

| Field | Type | Required | Validation | Description |
|-------|------|----------|------------|-------------|
| `payment_method` | String | Yes | Enum: `Stripe`, `VodafoneCash` | Distinguishes whether the payment is manual or automated. |
| `vodafone_number_shown` | String | No | Regex: `^010\d{8}$` | Snapshot of the teacher's Vodafone number shown to the student at confirmation time (for audit). |
| `receipt_image_url` | String | No | URL / File ID | Reference to the uploaded transaction receipt image bucket ID (used when `payment_method` == 'VodafoneCash'). |
| `reviewed_by` | String | No | Foreign Key (Users) | The ID of the teacher who approved or denied this manual attempt. |

*(Note: The existing `payment_status` column handles: `pending`, `success`, `failed`. `success` functionally equates to `Approved` and `failed` equates to `Denied` for manual flows.)*

## Access Control & Security
- The `receipts` bucket within Appwrite Storage requires strict permissions: read permissions must be strictly granted to the uploading student (`user_id`) and the teacher owning the course.
- Update permissions on `Payments` are strictly segmented: only teachers can change `payment_status` from `pending` to `success` or `failed`.
- The database logic will ensure that if a `Payments` record changes to `success`, it triggers the creation of the required `Subscriptions` record (as defined in the ERD).
