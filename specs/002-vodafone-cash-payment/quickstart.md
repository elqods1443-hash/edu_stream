# Quickstart: Vodafone Cash Payments

This document explains how to set up the environment and seed it with manual payment test data.

## Getting Started

1. Ensure your local Appwrite instance is running and seeded with basic Teacher and Student users.
2. Initialize the Collections specified in `data-model.md` in Appwrite via your project's Appwrite CLI migrations or Database web UI. Ensure `TeacherPaymentProfile`, `CoursePaymentSettings`, and `Enrollment` are fully operational.
3. Configure the `receiptImage` bucket within Appwrite Storage.

## Bootstrapping Data

You can start by running your test scripts or adding a dummy Vodafone number directly logic-wise in Appwrite:

```json
// Example Appwrite Enrollment JSON Payload
{
  "studentId": "student_123",
  "courseId": "course_abc",
  "status": "Pending",
  "paymentNumberShown": "01012345678",
  "receiptImageUrl": "BUCKET_ID/FILE_ID",
  "submissionDate": "2026-04-10T14:30:00Z"
}
```

## Running the Web Application

1. Make sure Redux Toolkit and DaisyUI are installed.
```bash
npm i @reduxjs/toolkit react-redux daisyui
```
2. Start your frontend development server:
```bash
npm run dev
```

## Development and Testing

- Look inside `src/features/pending-enrollments` to work on the teacher dashboard FSD slice.
- Test edge cases by deliberately mocking non `010` phone numbers.
