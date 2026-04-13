# My Bookmarks App

A personal bookmark dashboard built as a single HTML file. Data is persisted in the browser's `localStorage`, so no backend is required. Includes light/dark themes, category filtering, search, and edit/delete.

App lives at `myapps/my-bookmarks-app/` inside the parent repo.

## Running Locally

Three options, from simplest to most production-like. All commands assume you are at the repo root.

### 1. Open directly (no server)
```bash
open myapps/my-bookmarks-app/index.html
```

### 2. Python static server
```bash
cd myapps/my-bookmarks-app
python3 -m http.server 8000
# visit http://localhost:8000
```

### 3. Docker (matches production)
```bash
cd myapps/my-bookmarks-app
docker build -t my-bookmarks-app .
docker run --rm -p 8080:80 my-bookmarks-app
# visit http://localhost:8080
# stop with Ctrl+C
```

## Deploying to Coolify

**Prerequisites:** a running Coolify server, a GitHub (or Gitea) account, and the parent repo pushed to a remote.

### Step 1 — Push the parent repo
From the repo root (not the app subdirectory):
```bash
git add myapps/my-bookmarks-app
git commit -m "Add bookmark dashboard app"
git push origin main
```

### Step 2 — Create the Coolify application
Because the app is a subdirectory of a monorepo, Coolify needs to know where to build from.

1. Open the Coolify dashboard and select a Project.
2. Click **+ New Resource**.
3. Choose **Public Repository** (or **Private Repository** via the GitHub App).
4. Paste the parent repo URL and select branch `main`.
5. **Build Pack:** select **Dockerfile**.
6. **Base Directory:** `/myapps/my-bookmarks-app` ← important: points Coolify at the subdirectory so the Dockerfile's `COPY index.html` resolves correctly.
7. **Dockerfile Location:** `/myapps/my-bookmarks-app/Dockerfile`.
8. **Ports Exposes:** `80`.

### Step 3 — Domain & deploy
1. Under **Domains**, accept the generated Coolify subdomain or enter your own domain (add an A record pointing to the Coolify server first).
2. Coolify auto-provisions a Let's Encrypt certificate for custom domains.
3. Click **Deploy**.
4. Watch the build logs — the nginx container should start and report healthy.
5. Visit the domain to confirm the app loads.

### Step 4 — Continuous deployment (optional)
- Enable **Auto Deploy** so every push to `main` triggers a rebuild.
- For GitHub, connect via Coolify's GitHub App for webhook-based deploys.
- If the parent repo contains other apps, set a **Watch Path** of `myapps/my-bookmarks-app/**` so unrelated changes don't trigger a rebuild.

## Features
- Add, edit, and delete bookmarks (title, URL, category, notes)
- Live search by title or category
- Category filter pills
- Light/dark theme (preference persisted)
- Responsive grid layout
- Sample bookmarks on first load
