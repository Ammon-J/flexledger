# F4 — Delivery Block API

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §Block Routes.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F4 |
| **Section** | Backend API |
| **Severity** | MAJOR |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | S (1w) |
| **Owner (proposed)** | Team 1 |
| **Depends on** | F2 |
| **Unblocks** | F5, F8 |

---

## 1. Problem Statement

Drivers need a way to record specific delivery shifts, mileage, and earnings to calculate their tax write-offs. The system needs backend logic to securely save and retrieve this block data per user.

## 2. Goals

- Create the MongoDB Schema for Delivery Blocks.
- Implement `GET`, `POST`, `PUT`, and `DELETE` endpoints for `/api/blocks`.
- Ensure a user can only access their own block data.

## 3. Non-Goals

- Automated mileage calculation via GPS maps API (future improvement).

## 4. Personas & User Stories

- **As an Amazon Flex delivery partner**, I want to log my 3-hour shift details (start time, end time, miles driven, pay) so that my data is saved for tax season.

## 5. Functional Requirements

- **FR-1.** The system MUST associate every created block with the authenticated user's `_id`.
- **FR-2.** The system MUST allow a user to update an existing block.
- **FR-3.** The system MUST return `403 Forbidden` if a user attempts to modify a block they do not own.

## 6. Non-Functional Requirements

- **Security** — All routes MUST be protected by the JWT auth middleware from F2.

## 7. Acceptance Criteria

- **AC-1.** *Given* an authenticated user, *When* sending a `POST /api/blocks` with valid data, *Then* a 201 status and the new document are returned.
- **AC-2.** *Given* User A's token, *When* sending a `DELETE /api/blocks/:id` for User B's block, *Then* a 403 or 404 error is returned.

## 8. Data Model

- Collection: `blocks`
- Fields: `_id`, `userId` (ObjectId ref), `date` (Date), `startTime` (String), `endTime` (String), `milesDriven` (Number), `earnings` (Number).

## 9. API Surface

- `GET /api/blocks`
- `POST /api/blocks`
- `PUT /api/blocks/:id`
- `DELETE /api/blocks/:id`

## 10. UI / UX

N/A - Backend only.

## 11. AI / ML Considerations

N/A.

## 12. Integration Points

- MongoDB `blocks` collection.

## 13. Dependencies & Sequencing

- Must ship after: F2.
- Must ship before: F5.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Date/Time timezone discrepancies | H | M | Store all dates in UTC; let frontend handle local timezone offsets. |

## 15. Rollout Plan

- Database schema migration/initialization handled dynamically by Mongoose upon first insertion.

## 16. Test Plan

- **Integration** — Verify CRUD operations. Verify auth isolation between two different test users.

## 17. Documentation & Training

- Update API Docs with Block schema payload definitions.

## 18. Open Questions

1. Do we need to log the specific vehicle used for each block if the driver has multiple cars?

## 19. References

- `server/routes/blocks.js`.