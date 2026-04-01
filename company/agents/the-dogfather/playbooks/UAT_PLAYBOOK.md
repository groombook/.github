# UAT Playbook — GroomBook

CTO-owned test library. Used to create atomic UAT subtasks for Shedward. Shedward never reads this file directly.

## Known Fragile Areas

Track production escapes and areas that need extra scrutiny. Use this to prioritize deeper subtasks.

| Area | Defect | Issue | Root Cause | Extra Checks |
|------|--------|-------|------------|--------------|
| Portal Auth | Portal always showed "Hi, Guest" | GRO-300 | Dev session endpoint not creating portal sessions | Verify `browser_network_requests` for session API — must return 200, not 401/500 |
| Services Seed | Every service appeared twice | GRO-301 | Missing ON CONFLICT in seed script | Count service entries — must match expected count exactly |
| Reports | All reports showed "No data" | GRO-302 | UTC date handling in report queries | Verify with known date range that has data — must show non-empty charts |
| Landing Page | Dead-end "Please sign in" with no redirect | GRO-309 | No redirect/link when portal session missing | Verify unauthenticated portal redirects to /login |

**Rule:** After any production escape, add an entry here. When creating subtasks for that area, include the extra checks.

## Test Data

### Staff Accounts
| Name | Email | Role |
|------|-------|------|
| Jordan Lee | jordan@groombook.dev | Manager |
| Sam Rivera | sam@groombook.dev | Groomer |
| Sarah Mitchell | sarah@groombook.dev | Groomer |

### UAT Test Clients (impersonation only — clients cannot log in directly)
| Client | Email | Pet | Notes |
|--------|-------|-----|-------|
| UAT Test Alpha | uat-alpha@groombook.dev | TestBuddy (Golden Retriever) | Has pending invoice |
| UAT Test Bravo | uat-bravo@groombook.dev | TestMax (Labrador) | Has pending invoice |
| UAT Test Charlie | uat-charlie@groombook.dev | TestCooper (Poodle) | Has pending invoice |

### Environment
- **Dev URL:** https://groombook.dev.farh.net
- **Admin URL:** https://groombook.dev.farh.net/admin
- **Prod URL:** https://groombook.farh.net (NEVER test here)

### Navigation Rules
- Admin portal (`/admin/*`): URL navigation works.
- Customer portal (root `/`): SPA — click sidebar links only. Never type URL paths.

---

## TS-AUTH: Authentication

**Purpose:** Verify login, session management, and logout.

1. Navigate to https://groombook.dev.farh.net
2. PASS: Page loads without error
3. Log in as Jordan Lee (jordan@groombook.dev)
4. PASS: Admin dashboard loads, shows appointment data
5. Check browser_console_messages
6. PASS: No 500 errors, no unhandled JS exceptions
7. Check browser_network_requests
8. PASS: No 401 or 500 responses on API calls (session/auth endpoints must return 200)
9. Click logout (or sign out link)
10. PASS: Redirected to login page, session cleared
11. Log back in as Jordan Lee
12. PASS: Session restored, dashboard shows data

---

## TS-APPT: Appointments

**Purpose:** Verify appointment calendar CRUD.

1. Log in as Jordan Lee
2. Navigate to /admin/appointments
3. PASS: Calendar view loads with existing appointments
4. Click an existing appointment
5. PASS: Detail modal shows client, service, groomer, start/end, status, notes
6. Click "+ New Appointment" or Book
7. PASS: Booking wizard opens (Service → Date & Time → Info → Confirm)
8. Select a service, date, time slot, and client
9. PASS: Confirmation step shows correct details
10. (Optional) Submit booking
11. PASS: New appointment appears on calendar

---

## TS-CLIENT: Client Management

**Purpose:** Verify client CRUD, search, enable/disable.

1. Log in as Jordan Lee
2. Navigate to /admin/clients
3. PASS: Client list loads with multiple clients
4. Use search box — type "UAT Test Alpha"
5. PASS: Search filters to matching client(s)
6. Click on UAT Test Alpha
7. PASS: Client detail page shows name, email, pets, appointment history
8. Toggle "Show disabled" filter
9. PASS: Filter toggles correctly
10. Click "+ New" client button
11. PASS: Create client form opens

---

## TS-PET: Pet Management

**Purpose:** Verify pet profiles and associations.

1. Log in as Jordan Lee
2. Navigate to /admin/clients
3. Click UAT Test Alpha
4. PASS: Client detail shows TestBuddy (Golden Retriever)
5. Click on TestBuddy
6. PASS: Pet profile shows breed, grooming notes, visit history
7. (If available) Edit pet details
8. PASS: Changes save correctly

---

## TS-SERVICE: Services

**Purpose:** Verify service list, no duplicates, CRUD.

1. Log in as Jordan Lee
2. Navigate to /admin/services
3. PASS: Services list loads
4. PASS: No duplicate service entries (each service appears exactly once)
5. Check service details: name, price, duration visible
6. (If available) Click "+ New Service"
7. PASS: Create service form opens

---

## TS-STAFF: Staff Management

**Purpose:** Verify staff list, roles, super user controls.

1. Log in as Jordan Lee
2. Navigate to /admin/staff
3. PASS: Staff list shows all team members with roles
4. Click on a staff member
5. PASS: Detail page shows role, permissions, schedule
6. Check super user toggle
7. PASS: Toggle is visible and functional for manager accounts
8. Try deactivating a staff member
9. PASS: Deactivation guard prompts for confirmation

---

## TS-INVOICE: Invoicing

**Purpose:** Verify invoice list, creation, status workflow.

1. Log in as Jordan Lee
2. Navigate to /admin/invoices
3. PASS: Invoice list loads with date, client, subtotal, tax, tip, total, status
4. PASS: Shows both PAID and PENDING invoices
5. Click "View" on an invoice
6. PASS: Invoice detail opens with line items
7. Click "+ Create Invoice"
8. PASS: Invoice creation form opens

---

## TS-GROUP: Group Bookings

**Purpose:** Verify group booking functionality.

1. Log in as Jordan Lee
2. Navigate to /admin/group-bookings
3. PASS: Page loads (may show empty state or existing bookings)
4. Click "+ New Group Booking"
5. PASS: Group booking form opens with client dropdown, service/staff per slot

---

## TS-REPORT: Reports

**Purpose:** Verify reports show data for valid date ranges.

1. Log in as Jordan Lee
2. Navigate to /admin/reports
3. Set date range to cover last 30 days
4. PASS: Revenue by Day shows data (not "No data for this period")
5. PASS: Revenue by Groomer shows data
6. PASS: Appointment Trends shows data
7. PASS: Service Popularity shows data
8. PASS: Client Retention shows data
9. Change date range to a future period with no data
10. PASS: Reports correctly show "No data for this period"

---

## TS-SETTINGS: Settings / Branding

**Purpose:** Verify business settings page.

1. Log in as Jordan Lee
2. Navigate to /admin/settings
3. PASS: Settings page loads with business name, logo upload, color pickers
4. PASS: Preview reflects current settings
5. PASS: Save button is functional

---

## TS-PORTAL: Customer Portal

**Purpose:** Verify the full customer portal experience via impersonation.
**Fragile area:** Portal auth has escaped to prod before (GRO-300). Always include API verification.

1. Log in as Jordan Lee
2. Navigate to /admin/clients
3. Find UAT Test Alpha
4. Click "View as client" (impersonation)
5. PASS: Portal loads and shows client's name (NOT "Hi, Guest")
6. PASS: "STAFF VIEW" watermark visible (impersonation indicator)
7. Check browser_network_requests
8. PASS: Session/auth API calls return 200 (no 401, no 500)
9. Click "Appointments" in sidebar (do NOT type URL)
10. PASS: Appointments page loads
11. Click "My Pets" in sidebar
12. PASS: Shows TestBuddy (Golden Retriever)
13. Click "Billing" in sidebar
14. PASS: Shows at least one pending invoice
15. Click "Report Cards" in sidebar
16. PASS: Page loads (may be empty)
17. Click "Settings" in sidebar
18. PASS: Client settings page loads
19. Check browser_console_messages
20. PASS: No JS errors
21. Check browser_network_requests
22. PASS: No failed API calls across all portal pages
23. End impersonation
24. PASS: Returns to admin view

---

## TS-IMPERSONATE: Impersonation

**Purpose:** Verify impersonation start/end and audit trail.

1. Log in as Jordan Lee
2. Navigate to /admin/clients, find UAT Test Alpha
3. Click "View as client"
4. PASS: Portal loads with client context
5. PASS: "STAFF VIEW" watermark visible
6. Verify you see client-specific data (their name, pets, invoices)
7. End impersonation
8. PASS: Returns to admin, no residual client context
9. (If available) Check audit log for impersonation entry

---

## TS-BOOK: Public Booking Wizard

**Purpose:** Verify the multi-step booking flow.

1. Log in as Jordan Lee
2. Navigate to /admin/book (or the booking entry point)
3. PASS: Step 1 (Service selection) loads with service list
4. Select a service
5. PASS: Step 2 (Date & Time) loads with available slots
6. Select a date and time
7. PASS: Step 3 (Info) loads with client/pet fields
8. Fill in required info
9. PASS: Step 4 (Confirm) shows summary of all selections
10. (Optional) Submit booking
11. PASS: Confirmation displayed, no errors

---

## TS-SEARCH: Global Search

**Purpose:** Verify search across entities.

1. Log in as Jordan Lee
2. Use global search (if available) — search for "UAT Test Alpha"
3. PASS: Client result appears
4. Search for "TestBuddy"
5. PASS: Pet result appears
6. Search for a service name
7. PASS: Relevant results appear

---

## TS-SMOKE: Regression Smoke Test

**Purpose:** Quick pass across all admin sections and portal. Run after every deploy.

1. Log in as Jordan Lee
2. Click through each admin sidebar section:
   - Appointments → PASS: loads
   - Clients → PASS: loads
   - Staff → PASS: loads
   - Services → PASS: loads, no duplicates
   - Invoices → PASS: loads
   - Reports → PASS: loads
   - Settings → PASS: loads
3. Navigate to /admin/clients, find UAT Test Alpha, click "View as client"
4. PASS: Portal shows client name (not "Hi, Guest")
5. Click each portal sidebar link: Appointments, My Pets, Billing, Report Cards, Settings
6. PASS: Each loads
7. Check browser_console_messages
8. PASS: No JS errors
9. Check browser_network_requests
10. PASS: No 401/500 API responses across admin + portal navigation
11. End impersonation
12. PASS: Back to admin

---

## TS-PWA: PWA & Mobile Responsiveness

**Purpose:** Verify GroomBook works as a first-class PWA. GroomBook is NOT desktop-first — mobile/PWA is equally important.

### Mobile Viewport Tests

1. Resize browser to mobile viewport: `browser_resize` width=390, height=844 (iPhone 14)
2. Navigate to https://groombook.dev.farh.net
3. PASS: Login page is fully usable — no horizontal scroll, inputs visible
4. Log in as Jordan Lee
5. PASS: Admin dashboard renders cleanly at mobile width — no overflow, no cut-off content
6. Check sidebar navigation
7. PASS: Sidebar collapses to hamburger menu or stacks appropriately
8. Navigate to /admin/appointments
9. PASS: Calendar view adapts to mobile — scrollable or stacked, not clipped
10. Navigate to /admin/clients
11. PASS: Client list is scrollable, text readable, no horizontal overflow
12. Navigate to /admin/invoices
13. PASS: Invoice table is scrollable or stacked — all columns accessible
14. Navigate to /admin/reports
15. PASS: Charts resize to fit viewport, legends readable
16. Check browser_console_messages
17. PASS: No JS errors at mobile viewport

### Customer Portal — Mobile

18. Navigate to /admin/clients, find UAT Test Alpha, click "View as client"
19. PASS: Portal loads at mobile viewport — client name visible (not "Hi, Guest")
20. Click through portal sidebar links: Appointments, My Pets, Billing, Report Cards, Settings
21. PASS: Each page renders correctly at mobile width
22. Check browser_network_requests
23. PASS: No 401/500 API responses

### PWA Manifest & Installability

24. Resize browser back to desktop: `browser_resize` width=1280, height=720
25. Navigate to https://groombook.dev.farh.net
26. Check browser_network_requests for `/manifest.json` or `/manifest.webmanifest`
27. PASS: Manifest file loads (200 response)
28. Check browser_console_messages
29. PASS: No PWA-related warnings (missing icons, invalid manifest, etc.)

### Tablet Viewport (Optional)

30. Resize to tablet: `browser_resize` width=768, height=1024
31. Navigate through admin sections: Appointments, Clients, Services, Invoices
32. PASS: Layout adapts — not clipped, not tiny

---

## Standard Deploy Decomposition

When a PR deploys to dev, create these UAT subtasks:

| # | Subtask | Source | When |
|---|---------|--------|------|
| 1 | Environment readiness + API health | TS-AUTH steps 1-8 | Always first |
| 2 | Feature-specific test(s) | TS-{feature} | Based on PR scope |
| 3 | Portal smoke + API verification | TS-PORTAL steps 1-24 | Every deploy |
| 4 | Admin smoke test | TS-SMOKE steps 1-2 | Every deploy |
| 5 | Mobile viewport smoke | TS-PWA steps 1-17 | Every deploy |
| 6 | Portal mobile smoke | TS-PWA steps 18-23 | Every deploy |
| 7 | Console + network error audit | browser_console_messages + browser_network_requests | Every deploy |

Small PRs: 3-5 subtasks. Large PRs: 8-12 subtasks.

**Fragile area rule:** If the PR touches an area listed in Known Fragile Areas, add the extra checks from that table into the feature-specific subtask.

## Subtask Template

Use this format when creating UAT subtasks:

```
Title: UAT: [test area] — [what specifically]

Description:
## What
Test [feature area] after [PR/deploy context].

## Steps
[Numbered steps copied from playbook, customized with specific test data]

## Pass Criteria
[Explicit PASS conditions from the steps above]

## API Verification
After completing the steps, run browser_network_requests.
PASS: No 401, 403, or 500 responses on any API call.
If any API errors exist, this is a FAIL even if the UI looked correct.

## On PASS
Mark this issue done. Post a UAT PASS comment with what you tested.

## On FAIL
Set status to "todo", assign to CTO (2a556501-95e0-4e52-9cf1-e2034678285d).
Post what failed, steps to reproduce, expected vs actual, and attach a screenshot.
```
