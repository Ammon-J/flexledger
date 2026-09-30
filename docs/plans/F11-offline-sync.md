# F11 — Offline Sync & Conflict Resolution

> Implementation plan. Source: [docs/FlexLedgerMVP.md](../FlexLedgerMVP.md) §Offline Sync.

## Metadata

| Field | Value |
|---|---|
| **Feature ID** | F11 |
| **Section** | Full Stack Offline Logic |
| **Severity** | MAJOR |
| **Markets** | Independent Delivery Drivers |
| **Status (today)** | MISSING |
| **Estimated effort** | L (3w) |
| **Owner (proposed)** | Team 1 & 2 |
| **Depends on** | F5, F7, F10 |
| **Unblocks** | None |

---

## 1. Problem Statement

Being able to see the app offline (F10) is only half the battle. If a driver tries to submit a block while in a dead zone, the HTTP request will fail and data is lost. We need a way to queue these actions and sync them when the connection returns.

## 2. Goals

- Implement IndexedDB on the frontend to store failed `POST/PUT/DELETE` requests.
- Create a sync loop that checks for a network connection and flushes the queue.
- Implement backend deduplication to prevent double entries if network toggles rapidly.

## 3. Non-Goals

- Multi-device real-time sync conflicts (assuming user uses one device primarily).

## 4. Personas & User Stories

- **As a driver**, I want to hit "Save Shift" while in a dead zone and know the app will upload it automatically once I drive back to a main road.

## 5. Functional Requirements

- **FR-1.** The frontend MUST intercept failed API calls (due to `TypeError: Failed to fetch`) and save the payload to IndexedDB.
- **FR-2.** The system MUST automatically attempt to send queued payloads when the `online` window event fires.
- **FR-3.** The backend MUST utilize a unique idempotency key (generated on the client) to ignore duplicate requests.

## 6. Non-Functional Requirements

- **Reliability** — The queue must survive app restarts and browser crashes.

## 7. Acceptance Criteria

- **AC-1.** *Given* the device is offline, *When* a user submits a block, *Then* the UI shows "Saved locally" and queues the data.
- **AC-2.** *Given* locally queued data, *When* the network reconnects, *Then* the data is sent to the backend and cleared from the local queue.

## 8. Data Model

- **Backend:** Add `idempotencyKey` (String, unique index) to `blocks` and `maintenance` schemas.

## 9. API Surface

- Header `X-Idempotency-Key` added to `POST/PUT` requests.

## 10. UI / UX

- **Status Indicator:** A small cloud icon in the nav bar (Cloud with slash = offline; Cloud with check = synced; Cloud with arrow = syncing).

## 11. AI / ML Considerations

N/A

## 12. Integration Points

- Browser `IndexedDB` API (via a wrapper like `idb`).
- Browser `navigator.onLine` API.

## 13. Dependencies & Sequencing

- Must ship after: F5, F7, F10.
- Must ship before: None.

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| JWT token expires while data is queued offline | M | H | Check token validity before flushing queue; if expired, prompt user to login, then flush queue after successful auth. |

## 15. Rollout Plan

- Dogfood extensively. Simulate offline mode on physical devices.

## 16. Test Plan

- **End-to-end** — Playwright test toggling `offline` context, submitting form, toggling `online`, and verifying DB insertion.

## 17. Documentation & Training

- N/A

## 18. Open Questions

- Should we notify the user via push notification when an offline sync completes?

## 19. References

- `clients/web/src/utils/syncQueue.js`.