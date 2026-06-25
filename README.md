# JMS Hub

Single-page "mission control" that links to and embeds all of your live
dashboards in one place. Built as one static `index.html` file — no build
step, hosted on GitHub Pages exactly like JMS-Shipment-Tracker.

Each project opens **inside** the hub in a viewer panel (like Jarvis), with an
**Open in new tab** button to pop it out into its own browser window. Because
the panel loads each project's *live* URL, the hub always shows the latest
version of every dashboard automatically — when a project repo redeploys, the
hub reflects it on the next load. Nothing to sync.

---

## Adding / editing / removing a project

Open `index.html`, find the `PROJECTS` list near the bottom (inside the
`<script>` tag), and edit it. That's the only thing you ever need to change.

```js
const PROJECTS = [
  { name: "BoomRx Maps",             url: "https://boomrxmaps.com/" },
  { name: "JMS Shipment Tracker",    url: "https://flbocateam.github.io/JMS-Shipment-Tracker/index.html" },
  { name: "JMS Clinic Order Streak", url: "https://jms-metrics.github.io/Clinic-Ordering-History/" },
  { name: "JMS Sales Dashboard",     url: "https://flbocateam.github.io/JMS-Dashboard/" },

  // To add a new dashboard, copy a line and change the name + url:
  { name: "New Project",             url: "https://your-new-url.com/" },
];
```

Save, commit, push — the live hub updates within a minute. The sidebar grows
automatically; there is no limit.

---

## A note on embedding (important)

The in-page viewer uses an `<iframe>`. Some sites send security headers
(`X-Frame-Options` / `Content-Security-Policy: frame-ancestors`) that forbid
being embedded. Your GitHub Pages projects (`*.github.io`) allow it, so they
display inside the hub fine. If a project ever refuses to embed, the hub
detects it after a few seconds and shows a clean "open in new tab" fallback —
that project still works, it just opens in its own window.

---

## Deploy to GitHub Pages (mirrors JMS-Shipment-Tracker)

1. Create a new **public** repo under the `flbocateam` account, e.g.
   `jms-hub` (or name it `flbocateam.github.io` if you want it served at the
   root domain — see note below).
2. Add `index.html` and `README.md` to the repo root and push to `main`.
3. Repo **Settings → Pages → Build and deployment**:
   - Source: **Deploy from a branch**
   - Branch: **main** / **/ (root)** → Save
4. Wait ~1 minute. Your hub is live.

**Resulting URL**
- Repo named `jms-hub` → `https://flbocateam.github.io/jms-hub/`
- Repo named `flbocateam.github.io` → `https://flbocateam.github.io/` (cleanest
  "main hosting URL"; only one such repo allowed per account)

### Command-line version (if pushing from your machine)

```bash
# inside a folder containing index.html and README.md
git init
git add index.html README.md
git commit -m "Initial JMS Hub"
git branch -M main
git remote add origin https://github.com/flbocateam/jms-hub.git
git push -u origin main
# then enable Pages in Settings → Pages as above
```

### Custom domain (optional)

To serve the hub on your own domain (like BoomRx Maps does), add a `CNAME`
file to the repo root containing just the domain (e.g. `hub.boomrxmaps.com`),
then point that domain's DNS to GitHub Pages and set the domain under
Settings → Pages.
