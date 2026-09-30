# F3 — Auth Frontend UI

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §Frontend Auth.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F3 |
| **Section** | Frontend UI |
| **Severity** | BLOCKER |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | S (1w) |
| **Owner (proposed)** | Team 2 |
| **Depends on** | F2 |
| **Unblocks** | F5, F7, F8 |

---

## 1. Problem Statement

Users need a visual interface to interact with the authentication API. Without login and registration screens, drivers cannot access the protected ledger features.

## 2. Goals

- Build responsive login and registration forms using Tailwind CSS.
- Implement client-side JWT storage and session management.
- Route authenticated users to the main dashboard.

## 3. Non-Goals

- Complex animations or dark mode toggles (defer to post-MVP).

## 4. Personas & User Stories

- **As a driver**, I want to log into my account on my mobile phone quickly so I can start logging my shift before driving.

## 5. Functional Requirements

- **FR-1.** The frontend MUST display form validation errors (e.g., invalid email format) before submitting.
- **FR-2.** The system MUST store the received JWT securely (HTTP-only cookie or localStorage).
- **FR-3.** The frontend MUST redirect unauthenticated users away from protected pages to `/login`.

## 6. Non-Functional Requirements

- **Accessibility** — Forms must be fully navigable via keyboard and have appropriate `aria-labels` for screen readers.
- **Responsive** — Forms must be perfectly usable on 320px mobile screens.

## 7. Acceptance Criteria

- **AC-1.** *Given* a user is on `/login`, *When* they submit valid credentials, *Then* they are redirected to `/dashboard`.
- **AC-2.** *Given* an unauthenticated user, *When* they navigate to `/dashboard`, *Then* they are redirected to `/login`.

## 8. Data Model

- N/A - Client side state only.

## 9. API Surface

- Consumes `POST /api/auth/login` and `POST /api/auth/register`.

## 10. UI / UX

- **Pages:** `/login`, `/register`.
- **States:** Loading spinner during API call; red error text for incorrect credentials.
- **Mobile:** Full-width input fields.

## 11. AI / ML Considerations

N/A.

## 12. Integration Points

- Astro page routing.

## 13. Dependencies & Sequencing

- Must ship after: F2.
- Must ship before: F5.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| XSS vulnerabilities if token is in localStorage | M | M | Use DOM-purify and strictly avoid `set:html` in Astro. Consider HTTP-only cookies if time permits. |

## 15. Rollout Plan

- Deploy to preview environment for UI testing before GA.

## 16. Test Plan

- **End-to-end** — Playwright tests for user signup, login, and protected route redirection.
- **Accessibility** — Axe audit on form inputs.

## 17. Documentation & Training

- N/A.

## 18. Open Questions

1. Will we use `localStorage` or `cookies` for JWT storage in Astro?

## 19. References

- `clients/web/src/pages/login.astro`.