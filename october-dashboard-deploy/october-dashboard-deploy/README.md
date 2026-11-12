# October dashboard: deploy

Static site. No build step, no dependencies. Folder contents: `index.html`, `dashboards/` (archive), `vercel.json`, `netlify.toml`, `samples/`.

## Before you deploy: this page holds financial data
Anyone with the link can read it. The page asks search engines not to index it, but that is not protection.
Turn on password or login protection (Netlify: Site configuration > Access & security; Vercel: Project > Settings > Deployment Protection). Availability depends on your plan. Do not share the link publicly.

## Netlify
- Easiest: open https://app.netlify.com/drop and drag this whole folder in.
- CLI: `npm i -g netlify-cli`, then in this folder `netlify deploy --prod --dir .`

## Vercel
The Vercel website cannot take a zip or a dragged folder. Use one of these two routes.

**A. Command line (fastest).** Unzip, open a terminal in the folder that contains `index.html`, then:
1. `npx vercel login`
2. `npx vercel --prod`
3. Answer: set up and deploy? **Y**. Link to an existing project? **N** (or **Y** if you already made one with this name). Directory: **./** . If it asks to override settings, say **N**.

**B. GitHub.** Create a repository and upload the contents of this folder so `index.html` sits at the top level of the repo, not inside a subfolder. In Vercel: Add New > Project > import the repo. Framework Preset: **Other**. Leave Build Command and Output Directory empty. Deploy.

`vercel.json` already pins "no framework, no build, serve this folder", so a wrong preset can no longer break the deploy.

## If the deploy fails
- `npm run build exited with 1`, `No Output Directory named "dist" (or "public") found`, or `Missing script: build`: Vercel guessed a framework. Redeploy with this `vercel.json`, or set Framework Preset to Other.
- `404: NOT_FOUND` on the live URL: `index.html` isn't at the top of the project. Settings > General > Root Directory: set it to the folder that contains `index.html`.
- A Vercel login page, or `401`, instead of the dashboard: Deployment Protection is on. That is protecting your financial data. Sign in with the Vercel account on the team, or add the owner as a team member. Password Protection (no Vercel login needed) is a paid Vercel feature. Don't switch protection off.
- Deploy is "Ready" but the page is blank: open the browser console and send me the first red error.

## Updating the numbers
All figures live in one object, `const DATA = {...}`, near the top of the script in `index.html`. Colors are the `--brand` and `--accent` variables at the top of the style block.

## Monday brief
Click "Monday brief", then drop: a QuickBooks CSV (open invoices or bills), a PayPal activity CSV, a Google Calendar `.ics` export. Files are read in the browser and not uploaded. The last brief is saved in that browser only.
Try it with the files in `samples/` (fictional).
"Add Monday 7am reminder" downloads a weekly recurring calendar event as your nudge to drop the files.
This is drop-and-build, not automatic. Pulling from QuickBooks, Calendar and PayPal with no manual step needs a scheduled job with each service connected.

## Download and past months
- **Download** (top of the page) saves the dashboard you're viewing as one self-contained file, named like `dashboard-2026-10.html`. It opens in any browser with no internet needed except fonts.
- **Open** by month and year: pick a month and year, then Open. Months marked ● are available. Each dashboard is also saved in that browser the first time it loads, so anything you've opened is retrievable there.
- **Open a downloaded dashboard file** (below the pickers) loads any file saved with Download, from any device, and adds it to that browser's archive.
- **Archive on the site itself** (so everyone who visits sees past months, not just one browser): put each month's file in `dashboards/` as `YYYY-MM.html` and add a line to `dashboards/manifest.json`, e.g. `{ "2026-10": "/dashboards/2026-10.html", "2026-11": "/dashboards/2026-11.html" }`. The newest month should also be `index.html`.
- Browser-saved months stay on that device. Clearing site data removes them, so keep downloaded files as your real backup.
- The archive starts with October 2026. Earlier months don't exist yet because the source files only cover October.
