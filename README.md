# DMMHS Platform — Upgraded School Performance Dashboard

Extended version of the DMMHS Command Center dashboard. **All original charts, tables, KPIs, tabs, Chart.js logic, and default datasets are preserved.** New features are additive only.

## Quick start

**Windows:** Double-click `OPEN-DASHBOARD.bat` (or `OPEN-DASHBOARD.bat --https` for TLS)

```bash
cd C:\Users\kate\DMMHS-Platform
npm install
npm run generate-secrets   # copy output into .env
copy .env.example .env
npm start
```

Open **http://localhost:3847** (or **https://localhost:3847** with `--https`)

## Security (production)

1. Copy `.env.example` → `.env` and run `npm run generate-secrets`
2. Set `NODE_ENV=production`, `JWT_SECRET` and `SESSION_SECRET` (64+ chars each)
3. Set `HTTPS_KEY_PATH` and `HTTPS_CERT_PATH` to your TLS certificate files
4. Server **refuses to start** without valid secrets in production
5. JWT rotation: set `JWT_SECRET_PREVIOUS` to the old secret when rotating `JWT_SECRET`

Development without `.env` uses an ephemeral auto-generated JWT secret (with console warning). No hardcoded secrets remain in source code.

## Default accounts

| Email | Password | Role |
|-------|----------|------|
| admin@dmmhs.edu.ph | admin123 | Full access, Data Admin, users |
| editor@dmmhs.edu.ph | editor123 | Edit enrollment/dropout/repeater/strand cells |
| viewer@dmmhs.edu.ph | viewer123 | View only |

If the API server is not running, login still works in **offline demo mode** (role inferred from email: `admin`, `editor`, or `viewer`).

## What was added

- DMMHS logo branding and navy/gold theme
- SQLite database via Express API (`server/`)
- JWT authentication and role-based access
- Click-to-edit CRUD on Enrollment, Dropouts, Repeaters, and SHS Strands
- JSON import/export, Excel export, print view
- Smart Analytics tab (forecast + dropout risk charts)
- Activity Logs and User Management (admin)
- Toast notifications, modals, view animations
- Offline-first: localStorage remains the fallback

## Original tabs (unchanged behavior)

Dashboard · Enrollment · Dropouts · Repeaters · Teachers · SHS Strands · Reports · Data Admin · Settings

## Project layout

```
DMMHS-Platform/
  public/
    index.html      # Full dashboard (legacy + extensions)
    assets/
      dmmhs-logo.svg
  server/
    index.js        # Express API (HTTPS, Helmet, JWT)
    config/
      env.js        # Environment validation
      security.js   # JWT rotation, headers, cookies
    certs/          # Dev TLS (gitignored .pem files)
    db.js           # SQLite schema & seed
  .env.example
  OPEN-DASHBOARD.bat
```

## Data sync

- Edits save to `localStorage` immediately.
- With the server running, changes also sync to `server/dmmhs.db`.
- Cross-tab updates still work via the `storage` event on `dmmhs_master_data`.
