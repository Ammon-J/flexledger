# F5 — Delivery Block UI

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §Frontend UI.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F5 |
| **Section** | Frontend UI |
| **Severity** | MAJOR |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | M (2w) |
| **Owner (proposed)** | Team 2 |
| **Depends on** | F3, F4 |
| **Unblocks** | F11 |

---

## 1. Problem Statement

Drivers need an intuitive dashboard on their mobile devices to log and view their shift data at the end of their route. Without a UI, the Delivery Block API is inaccessible to end-users.

## 2. Goals

- Build the main `/dashboard` displaying a list of recent blocks.
- Create form components for adding and editing a delivery block.
- Implement delete confirmations to prevent accidental data loss.

## 3. Non-Goals

- Complex chart visualizations (saved for F8 Tax Aggregation).

## 4. Personas & User Stories

- **As a driver**, I want to tap an "Add Shift" button and quickly enter my miles and earnings so I can close the app and go home.

## 5. Functional Requirements

- **FR-1.** The dashboard MUST fetch and render the user's blocks from `GET /api/blocks` on load.
- **FR-2.** The system MUST clear the form and display a success toast upon successful submission.
- **FR-3.** The system MUST prompt the user for confirmation before executing a `DELETE` API call.

## 6. Non-Functional Requirements

- **Accessibility** — Date and number inputs must use proper HTML5 types (`type="date"`, `type="number"`) to trigger correct mobile keyboards.

## 7. Acceptance Criteria

- **AC-1.** *Given* the dashboard is loaded, *When* the user clicks "Delete" on a block and confirms, *Then* the block is removed from the UI and the database.
- **AC-2.** *Given* the user is offline (simulated), *When* they load the page, *Then* the UI should show an error state (until F11 is implemented).

## 8. Data Model

- N/A

## 9. API Surface

- Consumes endpoints built in F4.

## 10. UI / UX

- **Pages:** `/dashboard`, `/blocks/new`, `/blocks/edit/[id]`.
- **Empty State:** "No blocks logged yet. Tap + to add your first shift."
- **Loading State:** Skeleton loaders for the list view.

## 11. AI / ML Considerations

N/A.

## 12. Integration Points

- Astro layouts and UI components.

## 13. Dependencies & Sequencing

- Must ship after: F3, F4.
- Must ship before: F11.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Fat-finger data entry mistakes | M | M | Use robust HTML5 validation (min=0) for miles and earnings. |

## 15. Rollout Plan

- N/A

## 16. Test Plan

- **End-to-end** — Playwright tests: Create a block, verify it appears in the list, edit it, and delete it.

## 17. Documentation & Training

- N/A

## 18. Open Questions

- Should we paginate the dashboard list, or just show the last 30 days?

## 19. References

- `clients/web/src/pages/dashboard.astro`.