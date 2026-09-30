# FlexLedger Expanded MVP Schedule (5-Week Plan)

| Week | Feature ID & Name | Type / Label | Specific Tasks |
| :--- | :--- | :--- | :--- |
| **Week 1** | **F1: Project Foundation & DB Setup** | Infrastructure/supporting | Initialize Node/Express server; configure MongoDB Atlas connection; initialize Astro frontend with Tailwind CSS. |
| | **F2: User Authentication API** | Blocked by F1 | Create User schema; build `POST /api/auth/register` and `login` routes; implement JWT generation and auth middleware. |
| | **F3: Auth Frontend UI** | Blocked by F2 | Build responsive Login and Registration forms; implement client-side JWT storage and protected route logic. |
| **Week 2** | **F4: Delivery Block API** | Blocked by F2 | Create Block schema; implement CRUD backend routes (`GET`, `POST`, `PUT`, `DELETE` for `/api/blocks`) protected by auth middleware. |
| | **F5: Delivery Block UI** | Blocked by F4 | Build main dashboard layout; create form components for logging shifts, mileage, and fuel; implement fetch calls to Block API. |
| **Week 3** | **F6: Maintenance Record API** | Blocked by F2 | Create Maintenance schema; implement CRUD backend routes (`GET`, `POST`, `PUT`, `DELETE` for `/api/maintenance`). |
| | **F7: Maintenance UI** | Blocked by F6 | Build the maintenance log view; create form components for logging oil changes, repairs, and part costs. |
| **Week 4** | **F8: Tax Aggregation & Summary UI** | Blocked by F4 & F6 | Write MongoDB aggregation pipelines to calculate total deductible mileage and expenses; build the frontend summary dashboard. |
| | **F9: User Profile Settings** | Blocked by F2 | Build UI and hook up `GET/PUT /api/user/profile` to allow users to update their name, email, or default vehicle details. |
| **Week 5** | **F10: PWA Service Worker Foundation** | Infrastructure/supporting | Configure Astro PWA integration; cache static assets (HTML, CSS, JS) so the app loads without a cellular connection. |
| | **F11: Offline Sync & Conflict Resolution** | Blocked by F5, F7 & F10 | Implement IndexedDB to queue form submissions when offline; build backend logic to detect and merge duplicate sync entries upon reconnection. |