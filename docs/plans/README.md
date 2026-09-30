# FlexLedger MVP Implementation Sequence

### Recommended Implementation Order & Dependency Map

The implementation sequence is structured to minimize idle time between the frontend and backend teams. Foundational infrastructure and authentication must be stabilized first, as almost every other feature requires a secure user context and a working database connection.

### First Feature to Implement and Why

**F1: Project Foundation & DB Setup** must be implemented first. It establishes the base repository, the Astro frontend, the Express server, and the MongoDB Atlas connection. 
* **Why:** Without this baseline, developers cannot test their code in a shared environment. Implementing features before F1 forces teams to use mock data and temporary local setups, leading to massive merge conflicts and duplicated effort when the real environment is finally established.

### Implementation Risks & Bottlenecks

* **The Auth Bottleneck (F2):** F2 (User Authentication API) is the most critical dependency risk. It directly blocks F3, F4, F6, and F9. If the backend team delays F2, the frontend team will be completely stalled after F1, and the entire 5-week schedule will cascade into failure. 
* **Offline Complexity (F11):** F11 (Offline Sync & Conflict Resolution) is the most complex feature and depends on F5, F7, and F10. If the UI components (F5, F7) or the base Service Worker (F10) are delayed or buggy, F11 will be pushed out of the MVP timeframe. F10 should be started as early as possible (parallel to Auth) to ensure caching infrastructure is solid before adding sync logic.

### Implementation Sequence Table

| Order | Feature ID & Name | Depends on | Why this order |
| :--- | :--- | :--- | :--- |
| 1 | **F1** - Project Foundation & DB Setup | None | Establishes the core repository, DB connection, and server infrastructure. Unlocks all parallel development. |
| 2 | **F2** - User Authentication API | F1 | Core security layer. Blocks all other APIs. Delaying this paralyzes the backend team and stalls the frontend auth UI. |
| 3 | **F3** - Auth Frontend UI | F2 | Unlocks protected UI routes. Must follow F2 so the frontend can immediately test real JWT storage instead of writing disposable mock fetch logic. |
| 4 | **F10** - PWA Service Worker Foundation | F1 | Can be built in parallel with F2/F3. Must be deployed early so caching bugs are discovered and fixed long before the complex F11 offline sync is introduced. |
| 5 | **F4** - Delivery Block API | F2 | Core business logic. Unlocks the primary dashboard data. Executing out of order risks exposing unauthenticated endpoints. |
| 6 | **F6** - Maintenance Record API | F2 | Can be built in parallel with F4. Required early to ensure the data structures are ready for the Tax Aggregation queries (F8). |
| 7 | **F5** - Delivery Block UI | F3, F4 | Brings the core value of the app to life. Requires the F4 API to be fully stable so the frontend team isn't building forms for endpoints that change shape. |
| 8 | **F7** - Maintenance UI | F3, F6 | Completes the data entry workflows. Relies on the same protected routing logic established in F3. |
| 9 | **F8** - Tax Aggregation & Summary UI | F4, F6 | Requires both block and maintenance data APIs to be stable. Attempting this earlier will result in broken math and inaccurate database aggregation pipelines. |
| 10 | **F9** - User Profile Settings | F2, F3 | Low-priority, independent feature. Can be scheduled whenever frontend/backend developers have spare capacity after the core ledger is functioning. |
| 11 | **F11** - Offline Sync & Conflict Resolution | F5, F7, F10 | The highest-risk feature. Demands that all forms, APIs, and PWA caching rules are 100% complete and tested. If attempted too early, sync conflicts will be impossible to debug. |