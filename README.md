# CivicLens

**Turn local problems into action.**

<<<<<<< HEAD
CivicLens is a client-side prototype that turns a photo and a short description of a civic issue — a pothole, an overflowing bin, a broken streetlight — into a structured, categorized, department-routed report in seconds. Built for a 24-hour open innovation hackathon.

---

## How it works

1. **Spot it** — see a civic issue on your street.
2. **Report it** — upload a photo and add a short description (optional quick tags help the classifier).
3. **AI structures it** — the issue is classified into a category, priority, and responsible department, with an editable AI-generated description and recommended action.
4. **Track it** — the report gets a unique ID and appears in **My Reports** and the **Dashboard**, with live category/priority breakdowns.

### The "AI" classifier

The classification step is a **simulated model**, not a real ML/vision call — this keeps the app fully static with zero backend and zero API cost for the demo. It works in two stages, both implemented in `index.html`:

1. **Keyword matching** — the description text is scanned against a small rule table (`KEYWORD_RULES`) for terms like "pothole," "garbage," "streetlight," "leak," "tree," "drain," etc.
2. **Colour-sampling fallback** — if no keyword matches, the uploaded photo is drawn to an offscreen `<canvas>`, downsampled to 40×40, and its average RGB is used as a rough heuristic (`guessFromColor`) to guess the issue type.

The result maps to a category, department, and recommended action via `CATEGORY_MAP`, and a priority via simple rules in `priorityFor()`. Everything is reviewable and editable by the user before submission (the "Review your report" step).

This is intentionally documented in-code (see the comment above `ANALYZE_ISSUE()` in `index.html`) as a stand-in for a real vision/LLM API call — see **Roadmap** below.

---

## Tech stack

- **Plain HTML / CSS / JavaScript** — no framework, no bundler, no build step.
- **[Three.js](https://threejs.org/) r128** (loaded via CDN in `index.html`) — powers the animated 3D city-grid hero scene and the persistent low-opacity background scene that runs behind the whole page.
- **Browser `localStorage`** — the only data store. No backend, no database, no API keys.

Everything the app needs to run lives inside `index.html` (HTML structure, `<style>` block, and `<script>` block). It is a single self-contained file by design, which is why this repository does not split it into separate `.css`/`.js` files or introduce a framework — doing so would add build tooling for no functional benefit and risks breaking a page that already works end to end.

---

## Project structure

```
civiclens/
├── index.html      # The entire app: markup, styles, 3D scenes, router, report flow, dashboard, storage
├── package.json     # Local static-server scripts (no runtime dependencies — the app itself needs none)
├── .gitignore
└── README.md
```

No other source files exist because none are genuinely needed: there is no backend, no build pipeline, no environment configuration, and no separated modules to wire up. Files were deliberately **not** added just to pad out the repository (e.g. no unused `.env.example`, since the app reads zero environment variables today).

---

## Running it locally

No installation is required — you can simply open `index.html` directly in a browser.


CivicLens lets citizens report civic issues — potholes, garbage overflow, broken streetlights, water leaks — in seconds. Upload a photo and a short description, and the app automatically classifies it by category, priority, and department, producing an editable report you can track. Built for Hack Devengers 2.0, 2026.

> Hackathon prototype — runs entirely in the browser using `localStorage`. No backend, no sign-up, no data leaves your device.

## Features
- Instant photo + description reporting
- Auto-classification (category, priority, department)
- "My Reports" tracking with unique IDs
- Live dashboard with category/priority breakdowns

## Tech stack
Plain HTML/CSS/JS + [Three.js](https://threejs.org/) for the 3D hero scene. No framework, no build step.

## Run it
Just open `index.html` in a browser — or run:
 
```bash
npm install
npm start
```

>HEAD
This starts a static file server (via the [`serve`](https://www.npmjs.com/package/serve) package) on `http://localhost:5500`.

---

## Data & privacy

- Reports are stored under the `civiclens_reports_v1` key in the browser's `localStorage`.
- Uploaded photos are stored as base64 data URLs inside that same local record — they never leave the device.
- Clearing your browser's site data for this page removes all reports.

---

## Roadmap (beyond the prototype)

These are documented on the landing page itself ("Future possibilities") and are not implemented:

- **Direct department integration** — replace the local demo queue with a real API handoff into municipal ticketing systems.
- **Duplicate detection** — group repeat reports of the same issue into one tracked case.
- **Resolution verification** — request a follow-up photo to confirm a fix.
- **Public heatmaps** — ward-level views of where issues cluster.
- **Real AI classification** — swap the rule-based `ANALYZE_ISSUE()` function for an actual vision/LLM API call (this is the single function to replace; everything downstream — review step, storage, dashboard — already consumes its output shape and would not need to change).

---



