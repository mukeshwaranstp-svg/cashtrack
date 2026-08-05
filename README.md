# CashTrack — cloud deploy package

This folder is the **public-internet version** of CashTrack: the FastAPI backend
plus the already-built frontend (`dist/`). It is meant to be pushed to GitHub and
deployed to a free Render web service so the app gets a permanent HTTPS address —
which is the requirement for turning it into an installable Android APK.

## What's inside
- `cashtrack-backend/` — FastAPI app (`app/main.py`), serves the API *and* the
  frontend from `../dist` (same-origin, so the React app's `/api/*` calls work).
- `dist/` — the built frontend (run `npm run build` locally to refresh it).
- `render.yaml` — Render blueprint: build + start commands, DATABASE_URL env var.
- `Procfile` — fallback start command for Render web services.

## Deploy to Render (one time)
1. Push this folder to a GitHub repo.
2. Sign up / log in at https://render.com (free).
3. Dashboard → **New + → Web Service** → connect the GitHub repo.
4. Render auto-detects `render.yaml`. Create the service (Free plan).
5. In the service **Environment** tab, set `DATABASE_URL` to your Supabase
   Postgres connection string, e.g.
   `postgresql://postgres:PASSWORD@db.xxx.supabase.co:5432/postgres`
6. **Deploy**. When the build finishes you get a URL like
   `https://cashtrack.onrender.com`. Open it — the app is live on the internet.

Note: `cashtrack-backend/.env` and `certs/` are gitignored and stay local only.
On Render the settings come from the dashboard env vars.

## Turn it into an Android APK
1. Open the public URL in a browser and confirm the PWA loads.
2. Go to https://www.pwabuilder.com → enter your URL → **Package for Android**.
3. Download the generated APK and open it on your phone to install
   (enable "Install unknown apps" when prompted).
4. Optional: publish that same package to the Google Play Store ($25 one-time).

## Database
The default database is SQLite (local dev). On Render you must set
`DATABASE_URL` to Postgres — the code auto-normalizes Supabase URLs
(`postgresql://` → `postgresql+psycopg2://`).

## Refreshing the frontend after a change
Run `npm run build` in the original project, then copy the new `dist/` into this
folder, commit, and push — Render auto-deploys.
