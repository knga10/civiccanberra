# Your ACT Budget, Personally

A small static web app that turns the ACT Government's 2026–27 Budget into a
personal statement: answer a few questions about your household, and see
which Budget measures are likely to affect you, why, and how to read more.

**Live structure:** https://knga10.github.io/civiccanberra
---

## Why this exists

Government budget documents are written for everyone and no one. The
"ACT Budget at a Glance 2026–27" covers everything from stamp duty
concessions to basketball stadiums to bushfire mitigation — but most
people only care about the handful of measures that actually touch their
own life.

This tool takes your answers (renter vs owner, kids in public/private
school, region of Canberra, sports you follow, etc.) and scores ~45
headline Budget measures by relevance to *you*, then explains in plain
English why each one matters (or doesn't).

## How it works

- **`index.html`** — the whole app. A single-page React app (loaded via
  CDN, compiled in-browser with Babel — no build step). You answer four
  short sections of questions, and it generates:
  - An overall "relevance score"
  - A ledger of every Budget measure, sorted by relevance to you, each
    with a plain-English explanation
  - A "by area of the Budget" breakdown (Housing, Health, Education, etc.)
    with a short note on how those scores are calculated

- **`ACT-Budget-2026-27-at-a-glance.md`** — a markdown conversion of the
  official "ACT Budget at a Glance 2026–27" PDF, with the same headings
  and content, used as the underlying source text.

- **`budget-reference.html`** — the markdown above, rendered as a styled
  page with a named anchor on every section heading. Every measure in the
  main app links here (and to the official PDF), so you can read the full
  context behind any score.

## Why we built it this way

- **No backend, no build step.** Everything runs in the browser. React and
  Babel are loaded from a CDN and JSX is compiled on the fly. This means
  the whole thing is three static files that work on GitHub Pages, Vercel,
  or literally any static host — drop the files in a folder and it works.

- **Your answers stay on your device.** Profile data is stored in
  `localStorage` only. Nothing is sent to a server, logged, or shared.
  Refreshing the page (or coming back later on the same device/browser)
  restores your saved statement.

- **Transparent scoring.** Every measure's relevance score is calculated
  from a simple, readable rule based on your answers (e.g. "first home
  buyer → stamp duty exemption scores 100"). Tapping a ledger line shows
  the reasoning in plain language, so the scoring isn't a black box.

- **Real citations, not PDF page guesses.** Early versions linked to PDF
  page numbers, which are unreliable across browsers and PDF viewers.
  Instead, every measure links to a specific anchored section in
  `budget-reference.html` (a faithful markdown rendering of the official
  document), plus a link to the original PDF for anyone who wants the
  primary source.

## Important caveats

This is an **illustrative guide**, not financial, legal, or eligibility
advice. Relevance scores are heuristics based on the information you
provide — they're meant to help you find the parts of the Budget worth
reading more about, not to determine your actual entitlements. Always
check [act.gov.au](https://www.act.gov.au) or the official Budget
documents for eligibility criteria and exact figures.


## Updating for a future Budget

To reuse this for a future year's Budget:
1. Replace `ACT-Budget-2026-27-at-a-glance.md` with a markdown conversion
   of the new Budget at a Glance document, adding `<a id="...">` anchors
   to each heading you want to link to.
2. Regenerate `budget-reference.html` from that markdown.
3. Update the `BUDGET_ITEMS` array in `index.html` — figures, descriptions,
   relevance rules, and `refSection(...)` anchor references.
