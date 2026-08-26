# Fast One — website & admin panel

Front-end for Fast One Food Point: a customer ordering page and an admin panel
that talks to the backend at `https://fastonefoodpoint.duckdns.org`. Everything
here is plain HTML/CSS/JS in single files — no build step, no dependencies.
Open a file in a browser and it runs.

## What is what

| Path | What it is |
|---|---|
| `index.html` | **Admin panel, live version.** Logs in against the backend and holds a bearer token; this is the file that was wired up in "Connected frontend to secure DuckDNS backend". |
| `backup/site/index.html` | **Customer ordering page** — the menu, cart, and checkout ("Fast One - Order Online"). |
| `backup/admin/admin.html` | Standalone admin build that keeps working from `localStorage` when the backend is offline. |
| `backup/admin/admin-legacy.html` | The previous version of that standalone build, kept for reference. |
| `backup/automation/n8n-workflow.json` | The n8n workflow behind the ordering automation. Import it into n8n. |
| `backup/scans/`, `backup/photos/` | Scanned paperwork and photos kept with the project. |

`backup/` is the material that was previously in a folder named "fast one
project files that are to be used in case of emergency" — same files, same
purpose, names that a shell and a URL can both handle.

## Two things to decide

**1. The root of this repository is the admin panel, not the shop.** Nothing is
published today (GitHub Pages is off), so nothing is broken. But if Pages is
ever switched on, `index.html` is what the world gets — and that is the admin
login. If you want to publish the shop, the customer page should become the
root `index.html` and the admin panel should move to `admin/` (or, better, not
be published at all).

**2. `backup/admin/*.html` contain a fallback admin login in plain text** —
used when the backend is unreachable. Anyone reading this repository can read
it. See the note at the top of those files.

## Running it locally

```bash
python -m http.server 8000    # then open http://localhost:8000
```

Opening the files directly with `file://` also works; a local server only
matters if the browser blocks requests to the backend from `file://`.
