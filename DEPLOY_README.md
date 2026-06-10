# Margin — deploy guide

Static single-file web app (the prototype). No build step: Vercel serves `public/index.html` directly.

## Option A — deploy with the Vercel CLI (fastest)

From this folder, on a machine with internet:

```bash
npm i -g vercel        # if you don't have it
vercel login           # opens browser, sign in
vercel                 # first run: links/creates a project, deploys a preview
vercel --prod          # promotes to your production URL
```

That's it. The first `vercel` run asks a couple of questions (scope, project name) and prints the live URL.

## Option B — deploy via Git + Vercel git integration

If your Vercel project is connected to a Git repo (so pushes auto-deploy):

```bash
git init
git add .
git commit -m "Add Margin web app"
git branch -M main
git remote add origin <YOUR_GIT_REMOTE_URL>
git push -u origin main
```

Then in the Vercel dashboard, "Add New → Project", import this repo, and accept the defaults
(framework preset: "Other", output: leave empty — it serves `public/` as static).
Every push to `main` redeploys automatically.

## Files
- `public/index.html` — the entire app (React via CDN, no bundler needed)
- `vercel.json` — static config + basic security headers
- `.gitignore`

## Going from prototype to production
The app's data lives in the browser (localStorage) behind a single `db` object in `index.html`.
To wire it to Supabase, replace each `db.*` method body with a Supabase call — the `SUPABASE:`
comments in the file name the exact query each maps to. That work is unrelated to deployment;
the deploy above ships the current prototype as-is.
