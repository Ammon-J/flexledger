# F8 — Tax Aggregation & Summary UI

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §Tax Summaries.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F8 |
| **Section** | Full Stack Analytics |
| **Severity** | MAJOR |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | M (2w) |
| **Owner (proposed)** | Team 1 & 2 |
| **Depends on** | F4, F6 |
| **Unblocks** | None |

---

## 1. Problem Statement

The core value proposition of FlexLedger is simplifying tax calculations. Raw lists of blocks and maintenance are helpful, but drivers need a summarized view to quickly find their total annual deductions.

## 2. Goals

- Write an aggregation endpoint to calculate YTD total miles, block earnings, and maintenance costs.
- Build a Summary Dashboard UI to visualize these totals.
- Allow filtering by tax year.

## 3. Non-Goals

- Direct integration with IRS e-file or TurboTax.

## 4. Personas & User Stories

- **As a driver**, I want to see my total deductible mileage for the year multiplied by the IRS standard rate so I know my total deduction amount.

## 5. Functional Requirements

- **FR-1.** The backend MUST expose an endpoint `GET /api/summary?year=YYYY`.
- **FR-2.** The system MUST calculate standard mileage deduction (Total Miles * IRS Rate).
- **FR-3.** The frontend MUST display a comparison: "Standard Mileage Deduction vs. Actual Expenses Deduction" to help the user choose the best tax approach.

## 6. Non-Functional Requirements

- **Performance** — Aggregations on the DB level must not exceed 500ms for heavy users.

## 7. Acceptance Criteria

- **AC-1.** *Given* a user with 1000 miles driven in 2026, *When* they view the 2026 summary, *Then* the mileage deduction shows $670.00 (assuming a hypothetical $0.67 IRS rate).

## 8. Data Model

- N/A - Computed from existing `blocks` and `maintenance` collections.

## 9. API Surface

- `GET /api/summary?year=2026` returns `{ totalMiles, totalEarnings, totalMaintenance, estimatedDeduction }`.

## 10. UI / UX

- **Pages:** `/summary` or `/taxes`.
- **Components:** Large summary statistic cards (Tailwind `grid`).

## 11. AI / ML Considerations

N/A

## 12. Integration Points

- MongoDB Aggregation Framework.

## 13. Dependencies & Sequencing

- Must ship after: F4, F6.
- Must ship before: None.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| IRS standard mileage rates change yearly | H | M | Store the IRS rate as an environment variable or a configuration collection, NOT hardcoded. |

## 15. Rollout Plan

- QA phase to manually verify mathematical accuracy against mock datasets.

## 16. Test Plan

- **Unit** — Test math logic in the aggregation pipeline strictly.

## 17. Documentation & Training

- N/A

## 18. Open Questions

1. Should the user be able to manually override the IRS rate for specific edge cases?

## 19. References

- `server/routes/summary.js`.