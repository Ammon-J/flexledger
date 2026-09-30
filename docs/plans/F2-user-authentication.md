# F2 — User Authentication API

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §Auth Routes.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F2 |
| **Section** | Backend Authentication |
| **Severity** | BLOCKER |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | S (1w) |
| **Owner (proposed)** | Team 1 |
| **Depends on** | F1 |
| **Unblocks** | F3, F4, F6, F9 |

---

## 1. Problem Statement

Delivery drivers need their tax and shift data securely isolated from other users. Without a user authentication system, FlexLedger cannot associate blocks and maintenance records with specific drivers.

## 2. Goals

- Create a securely hashed User schema in MongoDB.
- Implement registration and login API endpoints.
- Create JWT-based authentication middleware to protect future routes.

## 3. Non-Goals

- OAuth (Google/Apple login) integration.
- Password reset via email workflow.

## 4. Personas & User Stories

- **As an Amazon Flex delivery driver**, I want to create a secure account so that my personal mileage data is kept private and safe.

## 5. Functional Requirements

- **FR-1.** The system MUST securely hash user passwords using bcrypt before saving to MongoDB.
- **FR-2.** The system MUST return a signed JWT upon successful login or registration.
- **FR-3.** The system MUST reject unauthorized requests to protected routes with a 401 status code.

## 6. Non-Functional Requirements

- **Security** — Passwords must never be logged or returned in API responses. JWTs must expire after 7 days.

## 7. Acceptance Criteria

- **AC-1.** *Given* a new user submits an email and password, *When* calling `POST /api/auth/register`, *Then* a user document is created and a JWT is returned.
- **AC-2.** *Given* an invalid token, *When* accessing a protected route, *Then* the server returns `401 Unauthorized`.

## 8. Data Model

- Collection: `users`
- Fields: `_id`, `email` (String, Unique), `passwordHash` (String), `createdAt` (Date).

## 9. API Surface

- `POST /api/auth/register` - Body: `{ email, password }`. Returns `{ token }`.
- `POST /api/auth/login` - Body: `{ email, password }`. Returns `{ token }`.

## 10. UI / UX

N/A - Backend API only.

## 11. AI / ML Considerations

N/A - Not AI-touching.

## 12. Integration Points

- Touches Node.js `crypto` or `bcrypt` modules.

## 13. Dependencies & Sequencing

- Must ship after: F1.
- Must ship before: F3, F4, F6.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| JWT secret exposed in code | L | H | Enforce strict `.env` usage and validate environment variables on server start. |

## 15. Rollout Plan

- Backend-only code merge. 

## 16. Test Plan

- **Integration** — Postman/Jest tests verifying successful login, invalid login (401), and duplicate registration (409).

## 17. Documentation & Training

- Update API documentation section in `FlexLedger.md`.

## 18. Open Questions

1. Should we enforce password complexity (e.g., 8+ chars)?

## 19. References

- Related plans: `../plans/F1-project-foundation.md`.