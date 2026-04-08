# CarTracker

A personal car expense tracking web app built with Flask. Track fuel, repairs, tolls, insurance, and gadgets across multiple cars — with multi-user sharing, live currency conversion, charts, and email expiry reminders.

![Python](https://img.shields.io/badge/Python-3.12-blue)
![Flask](https://img.shields.io/badge/Flask-3.0-green)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-purple)

---

## Features

### Expense Tracking
- **5 expense types:** Fuel, Repair, Toll, Insurance, Gadget
- **Multi-currency support** with live exchange rates (via [Frankfurter API](https://www.frankfurter.app/)) and real-time CZK conversion hint while filling in the form
- All amounts stored in original currency + converted CZK for consistent charts and totals
- **Fuel-specific fields:** liters, odometer, price/liter (auto-calculated), fuel station location, transaction time
- **Toll & Insurance expiry dates** with optional email reminders (configurable 1–30 days in advance)
- Notes field on every expense

### Fuel Station Autocomplete
Start typing a fuel station name and the app suggests stations from your previous entries across all your cars.

### Charts & Analytics (per car)
- Monthly spending by category (stacked bar)
- Expense breakdown by type (doughnut)
- **Fuel efficiency** (L/100 km) over time — filterable by date range, with overall average (total litres ÷ total km)

### Dashboard
- Spending totals, expense count, and this month's spend at a glance
- Quick **Add Expense** button — goes directly to the form if you have one active car, or shows a car picker modal if you have several
- Monthly spending chart and category breakdown

### Multi-User & Car Sharing
- Users can **own** cars (full control) or be **shared** on cars (add own expenses, edit/delete only their own)
- Car owners can share/unshare with any registered user
- Per-user car visibility: mark any car as **not used** to hide it from your own dashboard without affecting other users
- Owners can additionally hide a car from **all shared users** at once

### Admin
- Separate admin role for user management (create, edit, delete users)
- Admin overview of all expenses across all users and cars, with filtering by user, car, and expense type
- Admins cannot own cars or expenses — strictly a management role

### Filters & Export
- Filter expenses by car and/or expense type on both the user expense list and the admin overview
- **Export to Excel** (.xlsx) — respects active filters so you export exactly what you see
- Expiring/expired toll and insurance highlighted with warning banners on the car detail page

### UX
- **Dark mode** — follows your OS preference on login/setup; toggleable per-user when logged in (preference saved in DB)
- Fully responsive — works on mobile
- Light/dark-aware table headers, badges, and charts

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python 3.12, Flask 3 |
| Database | SQLite (dev) / PostgreSQL (production) |
| ORM & migrations | Flask-SQLAlchemy, Flask-Migrate (Alembic) |
| Auth | Flask-Login, Werkzeug password hashing |
| Forms & CSRF | Flask-WTF |
| Email | Flask-Mail (generic SMTP) |
| Scheduling | APScheduler (daily expiry notification job) |
| Frontend | Bootstrap 5.3, Bootstrap Icons, Chart.js 4 |
| Export | openpyxl |
| Exchange rates | [Frankfurter API](https://www.frankfurter.app/) (1-hour cache) |
| Deployment | Railway (Procfile + railway.toml) |

---

## Authorization Summary

| Action | Owner | Shared User | Admin |
|---|---|---|---|
| View car & expenses | ✅ | ✅ | ✅ |
| Add expense | ✅ | ✅ | ❌ |
| Edit / delete own expense | ✅ | ✅ | ❌ |
| Edit / delete others' expenses | ✅ | ❌ | ❌ |
| Edit / delete car | ✅ | ❌ | ❌ |
| Manage shares | ✅ | ❌ | ❌ |
| Manage users | ❌ | ❌ | ✅ |
| Hide car globally for shared users | ✅ | ❌ | ❌ |
