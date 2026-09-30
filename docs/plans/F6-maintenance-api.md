# F6 — Maintenance Record API

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §Maintenance Routes.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F6 |
| **Section** | Backend API |
| **Severity** | MAJOR |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | S (1w) |
| **Owner (proposed)** | Team 1 |
| **Depends on** | F2 |
| **Unblocks** | F7, F8 |

---

## 1. Problem Statement

Beyond mileage, drivers incur direct vehicle expenses (oil changes, tires, repairs) which are deductible. The system needs an API to record and organize these maintenance expenditures.

## 2. Goals

- Create the MongoDB Schema for Maintenance records.
- Implement `GET`, `POST`, `PUT`, `DELETE` endpoints for `/api/maintenance`.

## 3. Non-Goals

- Photo uploads for physical receipts (deferred).

## 4. Personas & User Stories

- **As a driver**, I want to log a $60 oil change expense so that it is included in my annual vehicle costs.

## 5. Functional Requirements

- **FR-1.** The system MUST associate maintenance records with the authenticated user.
- **FR-2.** The API MUST enforce a `type` enum for expenses (e.g., "Oil Change", "Tires", "Repairs", "Other").
- **FR-3.** The API MUST require a numeric `cost` value greater than 0.

## 6. Non-Functional Requirements

- **Reliability** — Ensure strict input validation using a library like Zod to prevent malformed data insertion.

## 7. Acceptance Criteria

- **AC-1.** *Given* a POST request with invalid expense type, *When* submitted, *Then* the server returns `400 Bad Request`.
- **AC-2.** *Given* a valid GET request, *When* processed, *Then* the server returns an array of the user's maintenance objects.

## 8. Data Model

- Collection: `maintenance`
- Fields: `_id`, `userId` (ObjectId), `date` (Date), `type` (Enum String), `description` (String), `cost` (Number).

## 9. API Surface

- `GET /api/maintenance`
- `POST /api/maintenance`
- `PUT /api/maintenance/:id`
- `DELETE /api/maintenance/:id`

## 10. UI / UX

N/A

## 11. AI / ML Considerations

N/A

## 12. Integration Points

- MongoDB `maintenance` collection.

## 13. Dependencies & Sequencing

- Must ship after: F2.
- Must ship before: F7.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Missing common maintenance types | M | L | Include a fallback "Other" type with a required description field. |

## 15. Rollout Plan

- Database updates apply on next merge.

## 16. Test Plan

- **Integration** — Test validation logic (e.g., negative cost numbers rejected).

## 17. Documentation & Training

- Update Postman collection.

## 18. Open Questions

1. Do we need an odometer reading field attached to maintenance records?

## 19. References

- `server/routes/maintenance.js`.