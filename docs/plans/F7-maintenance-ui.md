# F7 — Maintenance UI

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §Frontend UI.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F7 |
| **Section** | Frontend UI |
| **Severity** | MAJOR |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | S (1w) |
| **Owner (proposed)** | Team 2 |
| **Depends on** | F3, F6 |
| **Unblocks** | F11 |

---

## 1. Problem Statement

Drivers need an interface to manage their vehicle repair costs. Without this UI, the Maintenance API is unused and drivers miss out on logging non-mileage deductions.

## 2. Goals

- Build the `/maintenance` view to list past records.
- Create form components for adding/editing a maintenance log.

## 3. Non-Goals

- Depreciating asset value calculations.

## 4. Personas & User Stories

- **As a driver**, I want to look at a list of all my car repairs this year so I can see how much I've spent on upkeep.

## 5. Functional Requirements

- **FR-1.** The UI MUST provide a dropdown selector for the maintenance `type`.
- **FR-2.** The UI MUST format the `cost` display as local currency (e.g., $USD).
- **FR-3.** The UI MUST allow editing of existing records.

## 6. Non-Functional Requirements

- **Accessibility** — Form focus order should be logical (Date -> Type -> Description -> Cost -> Submit).

## 7. Acceptance Criteria

- **AC-1.** *Given* the user is on the maintenance log page, *When* they add an expense of 50.5, *Then* the list displays it formatted as "$50.50".

## 8. Data Model

N/A

## 9. API Surface

- Consumes `GET /api/maintenance` and related CRUD routes.

## 10. UI / UX

- **Pages:** `/maintenance`, `/maintenance/new`.
- **Navigation:** Add a tab/link in the main navigation menu to switch between "Blocks" and "Maintenance".

## 11. AI / ML Considerations

N/A

## 12. Integration Points

- Astro layout system.

## 13. Dependencies & Sequencing

- Must ship after: F3, F6.
- Must ship before: F11.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| UI clutter on mobile screens | H | M | Use list cards instead of wide HTML tables for mobile responsiveness. |

## 15. Rollout Plan

- N/A

## 16. Test Plan

- **Manual exploratory** — Test currency formatting edge cases (e.g., trailing zeros).

## 17. Documentation & Training

- N/A

## 18. Open Questions

None.

## 19. References

- `clients/web/src/pages/maintenance/index.astro`.