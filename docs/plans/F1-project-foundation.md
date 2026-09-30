# F1 — Project Foundation & DB Setup

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §Infrastructure.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F1 |
| **Section** | Infrastructure/Supporting |
| **Severity** | BLOCKER |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | S (1w) |
| **Owner (proposed)** | Team 1 (Backend) & Team 2 (Frontend) |
| **Depends on** | None |
| **Unblocks** | F2, F10 |

---

## 1. Problem Statement

FlexLedger currently lacks a foundational codebase and infrastructure. Delivery drivers need a reliable, hosted environment where the backend can process data and the frontend can render views. Establishing this ensures all subsequent features have a working environment to build upon.

## 2. Goals

- Initialize a Node.js/Express backend server.
- Establish a connection to a MongoDB Atlas cluster.
- Initialize an Astro frontend project with Tailwind CSS.

## 3. Non-Goals

- Implementing authentication routes or UI.
- Setting up CI/CD pipelines (out of scope for this week).

## 4. Personas & User Stories

- **As a developer**, I want an initialized repository with a database connection so that I can begin implementing feature branches.

## 5. Functional Requirements

- **FR-1.** The backend MUST connect to MongoDB Atlas using the `MONGO_URI` environment variable.
- **FR-2.** The backend MUST start an Express server on a configured port.
- **FR-3.** The frontend MUST compile Astro files and apply Tailwind CSS utility classes.

## 6. Non-Functional Requirements

- **Performance** — Astro build should take <30s locally.
- **Security** — `MONGO_URI` must be excluded from version control via `.gitignore`.
- **Maintainability** — Code formatted with standard Prettier configurations.

## 7. Acceptance Criteria

- **AC-1.** *Given* the repository is cloned, *When* `npm run dev` is executed, *Then* the Astro frontend and Express backend both start without errors.
- **AC-2.** *Given* the backend server starts, *When* initialized, *Then* the console logs a successful MongoDB Atlas connection.

## 8. Data Model

- N/A - No collections created yet.

## 9. API Surface

- `GET /api/health` - Returns `{ status: "ok" }` to verify server uptime.

## 10. UI / UX

- Default Astro landing page modified to display "FlexLedger API Connected".

## 11. AI / ML Considerations

N/A - Not AI-touching.

## 12. Integration Points

- MongoDB Atlas (M0 Free Tier).

## 13. Dependencies & Sequencing

- Must ship after: None.
- Must ship before: F2, F10.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| MongoDB IP Whitelisting blocks local dev | H | H | Document setting Network Access to `0.0.0.0/0` for development in README. |

## 15. Rollout Plan

- Feature flag name: N/A.
- Merge `main` to trigger baseline infrastructure deployment.

## 16. Test Plan

- **Unit** — Test DB connection utility.
- **Integration** — Verify `/api/health` returns 200 OK.
- **Manual exploratory** — Run setup instructions locally to verify developer onboarding.

## 17. Documentation & Training

- Update `README.md` with local `.env` setup instructions.

## 18. Open Questions

1. Which hosting provider will host the Express API in production?

## 19. References

- Existing files touched: `package.json`, `.env.example`, `astro.config.mjs`.