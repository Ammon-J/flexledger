# F9 — User Profile Settings

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §User / Account Routes.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F9 |
| **Section** | Full Stack UI/API |
| **Severity** | MINOR |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | S (1w) |
| **Owner (proposed)** | Team 1 & 2 |
| **Depends on** | F2, F3 |
| **Unblocks** | None |

---

## 1. Problem Statement

Users need a way to manage their account details, such as updating their password or deleting their account to comply with data privacy standards.

## 2. Goals

- Implement `GET` and `PUT` for `/api/user/profile`.
- Implement `DELETE /api/user/account`.
- Build the Frontend Settings Page.

## 3. Non-Goals

- Multi-vehicle garage management (sticking to one primary vehicle for MVP).

## 4. Personas & User Stories

- **As a driver**, I want to log my default vehicle (e.g., 2017 Ford Fusion SE AWD) so that I don't have to re-type it for every record.
- **As a user**, I want to delete my account permanently if I stop driving for Amazon Flex.

## 5. Functional Requirements

- **FR-1.** The system MUST allow users to update their profile information.
- **FR-2.** The system MUST permanently delete the user document AND all associated `blocks` and `maintenance` records upon account deletion.
- **FR-3.** The UI MUST prompt a final "type DELETE to confirm" modal before wiping data.

## 6. Non-Functional Requirements

- **Privacy & Compliance** — Deletion must cascade properly to adhere to GDPR/CCPA "right to be forgotten".

## 7. Acceptance Criteria

- **AC-1.** *Given* an authenticated user clicks Delete Account and confirms, *When* processed, *Then* all their documents are removed from MongoDB and they are logged out.

## 8. Data Model

- Add `vehicleDetails` (String) to `users` collection.

## 9. API Surface

- `GET /api/user/profile`
- `PUT /api/user/profile`
- `DELETE /api/user/account`

## 10. UI / UX

- **Pages:** `/settings`.

## 11. AI / ML Considerations

N/A

## 12. Integration Points

- MongoDB transaction/cascade delete.

## 13. Dependencies & Sequencing

- Must ship after: F2, F3.
- Must ship before: None.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Orphaned data on delete | M | M | Use Mongoose `pre('remove')` hooks or explicit deletion queries for associated collections. |

## 15. Rollout Plan

- N/A

## 16. Test Plan

- **Integration** — Verify database is fully clear of user records after deletion endpoint is hit.

## 17. Documentation & Training

- N/A

## 18. Open Questions

None.

## 19. References

- `server/routes/user.js`.