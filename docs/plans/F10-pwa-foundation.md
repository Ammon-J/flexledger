# F10 — PWA Service Worker Foundation

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §PWA Support.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F10 |
| **Section** | Frontend Infrastructure |
| **Severity** | MAJOR |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | M (2w) |
| **Owner (proposed)** | Team 2 |
| **Depends on** | F1 |
| **Unblocks** | F11 |

---

## 1. Problem Statement

Delivery routes often take drivers into rural areas or apartment complexes with cellular dead zones. If the web app requires a constant connection, drivers cannot reliably log their shifts at completion.

## 2. Goals

- Configure Astro to generate a `manifest.json`.
- Implement a Service Worker to cache the application shell (HTML, CSS, JS).
- Enable the app to be "installed" to the user's home screen.

## 3. Non-Goals

- Complex background sync API usage (using IndexedDB approach instead).

## 4. Personas & User Stories

- **As a driver**, I want to open the FlexLedger app even when I have zero bars of signal so that I can look at the interface.

## 5. Functional Requirements

- **FR-1.** The application MUST register a Service Worker on load.
- **FR-2.** The system MUST cache static assets upon initial load.
- **FR-3.** The app MUST serve cached UI shells when the network is `offline`.

## 6. Non-Functional Requirements

- **Reliability** — Cache invalidation must trigger correctly when a new app version is deployed.

## 7. Acceptance Criteria

- **AC-1.** *Given* the app has been loaded once, *When* the browser network is set to offline and the page is refreshed, *Then* the dashboard UI loads successfully without the dinosaur game screen.

## 8. Data Model

- N/A

## 9. API Surface

- N/A

## 10. UI / UX

- Install prompt/banner (e.g., "Add FlexLedger to Home Screen").

## 11. AI / ML Considerations

N/A

## 12. Integration Points

- `@astrojs/pwa` or Google Workbox.

## 13. Dependencies & Sequencing

- Must ship after: F1.
- Must ship before: F11.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Aggressive caching prevents users from seeing app updates | H | H | Configure Workbox to use `NetworkFirst` or `StaleWhileRevalidate` for HTML files. |

## 15. Rollout Plan

- N/A

## 16. Test Plan

- **Manual exploratory** — Use Chrome DevTools to simulate Offline mode and audit PWA compliance using Lighthouse.

## 17. Documentation & Training

- N/A

## 18. Open Questions

- What icons will we use for the PWA manifest?

## 19. References

- `astro.config.mjs`, `public/manifest.json`.