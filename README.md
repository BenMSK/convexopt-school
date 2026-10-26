# Convex Optimization School

A three-week course on convex optimization in plain English. Weeks 1–2 follow the Korean open textbook
[모두를 위한 컨벡스 최적화](https://convex-optimization-for-all.github.io/) (CO4A, MIT License), rephrased and simplified.
Week 3 goes beyond convexity: nonlinear optimization, MPC, trajectory optimization, collision avoidance, safety filters,
and solvers, as used in robotics and autonomous driving (as of October 2026).

The course is one self-contained web page with 21 lessons, 26 interactive widgets, 91 inline diagrams, 171 quiz questions,
17 algorithm cards, 15 robotics papers explained three ways, and a 140-term searchable glossary. Source links back every claim.

| Where | URL | Notes |
|---|---|---|
| GitHub Pages | https://benmsk.github.io/convexopt-school/ | Public. Progress (days done) and theme are saved only in the browser. Add it to the phone home screen for an app icon. |
| Repo | https://github.com/BenMSK/convexopt-school | Branch `main`; Pages serves the root. |
| Local | double-click `index.html` | Works offline except Google Fonts. |

## Files

```
ConvexOpt_school/
├─ research/NN_<slug>.md, papers_[abc].md   ← research notes per day (sources, verified numbers, cautions). Not published.
└─ school/                     ← git root, the published folder
   ├─ index.html               ← the whole site: CSS + content + script. The only source.
   ├─ apple-touch-icon.png     ← iOS home-screen icon 180px (tilted level-set contours around an amber minimum)
   ├─ icon-192.png, icon-512.png, manifest.webmanifest   ← Android / PWA icon and manifest
   ├─ .nojekyll                ← GitHub Pages serves files as-is
   ├─ tools/htmlcheck.js       ← tag balance, quiz answers in range, every data-viz has a VIZ function, no duplicate ids
   ├─ tools/review.py          ← SVG label audit + screenshots of one lesson/entry   python tools/review.py d8 [dark] [noshot]
   ├─ tools/makeicon.js        ← icon rasterizer       node tools/makeicon.js .
   └─ README.md
```

## Inside index.html

- `<head>`: meta, icons, manifest, Google Fonts (Inter, Space Grotesk, JetBrains Mono), and one `<style>` block in the "Lab" skin.
  Design tokens live in `:root` (light) and are redefined for dark mode twice: under `@media (prefers-color-scheme: dark)`, guarded by `:root:not([data-theme="light"])`, and under `:root[data-theme="dark"]` for the toggle.
- `<header class="top">`: brand, version `v1.x (date)`, theme button, and four tabs: Course · Algorithms · Papers · Glossary.
- `.pane[data-pane=course]`: day chips (built by script), `#d0` overview (hero, how-to, a notation table, the syllabus built from the day headers, box legend, sources), then `section.day[data-day=1..21]`.
- `.pane[data-pane=algos]` and `.pane[data-pane=papers]`: library panes. Cards are built from `article.entry#a-…` / `#p-…` (`data-year` sorts: the day number for algorithms, the publication year for papers; `data-tags` drives the filter chips).
- `.pane[data-pane=glossary]`: `section.qa-group` > `details.qa[data-cat]`, with search and category chips. The `data-go` buttons jump to a day.
- `<script>`:
  - config and UI: routing (`#d7`, `#algos`, `#a-admm`, `#p-gcs`, `#glossary`), theme, progress (localStorage key `convexopt-school`), quiz handling, and table-to-card labels for phones
  - helpers: `h`, `sv`, `txt`, `range`, `select`, `stat`, `lineChart`, `stepper`, `rng`/`randn` (seeded), `solve` (small dense linear solve), and `plane` + `contour` (2-D plots with level sets)
  - the `VIZ` registry: one function per widget name, mounted on every `div.viz[data-viz=name]`
  - glossary search, and `fitText` (shrinks or halos SVG labels that would overflow)

### Content conventions

- **Lesson structure:**
  1. `header.day-h`: eyebrow "Week W · Day N", title, a lead ("After today you can…", which feeds the syllabus), and a meta line with the CO4A chapters covered
  2. `.box.key`, the big idea
  3. 4–7 numbered `h3` sections, each with widgets (`div.viz`, preceded by "Try it:" and followed by a `p.cap` caption) and inline SVG figures (`figure.fig`, CSS-variable colours only)
  4. `.box.tip` (analogy), `.box.kid` (explain it to a 10-year-old), `.box.dont` (common misunderstandings), `.box.note` (going deeper, optional)
  5. `.quiz` with 6 `.q[data-a]` questions, four options each and a hidden `.exp`
  6. `.box.ok` recap, then `.refs`, then `p.next`
- **Writing:** each idea follows the same order: an everyday picture or a tiny 2-D example, then the general statement, then the formula, then one worked number. Proofs are optional. Math is HTML and Unicode only, with no LaTeX; display formulas go in `p.math`.
- **Sources:** every claim worth checking carries a chip `<a class="src" href="…">CO4A 6-2</a>`, `Boyd & Vandenberghe 2004`, `CMU 10-725 2016`, `Stellato 2020`, and so on. CO4A section pages are linked individually. The F16 CMU course was taught by Javier Peña and Ryan Tibshirani. Fast-moving facts are marked "as of Oct 2026".
- **CO4A errata:** where CO4A has a typo or an error, the lesson gives the corrected form. Many lessons have a "Reading CO4A alongside?" note listing the fixes. The research notes record each one.
- **Widgets:** each is a small real computation, not an animation, with seeded randomness. The numbers in each caption are what the widget's default state prints. The numbers were checked headlessly in Edge and against the research notes' node and numpy runs.
- **Ids are permanent once published:** `d1…d21`, the algorithm cards `a-gd a-sgd a-subgrad a-ista a-fista a-newton a-bfgs a-barrier a-pdipm a-alm a-admm a-fw a-cd a-bnb a-sqp a-lm a-scp`, and the papers `p-lossless p-footstep p-trajopt p-scvx p-sesync p-osqp p-malyuta p-iris p-cbf p-optnet p-cvxpylayers p-obca p-gcs p-safeboxes p-cupdlp`.

## Edit and publish

1. Edit `index.html` directly. For a new day, append `section.day` with the next number, then update the hero facts and the tab label "Course · N days".
2. `node tools/htmlcheck.js index.html` must print `errors: 0`.
3. `python tools/review.py dN` for every lesson or entry you touched; look at the PNGs it prints (written to `../build/review/`, outside the published folder).
4. Bump the version in the header and footer, and add a change-log line below.
5. `git commit`, then `git push` (GitHub Pages serves the main branch root).

## Change log

| Date | Version | Tag | What |
|---|---|---|---|
| 2026-10-26 | v1.1 | icon | New logo and home-screen icons: three tilted contour rings around the minimum, replacing the bowl. `tools/makeicon.js` gains a rotated `ring` shape. |
| 2026-10-26 | v1.0 | first release | 21 days (Week 1 foundations: CO4A ch 1–9 · Week 2 duality and algorithms: ch 10–23 · Week 3 MIP (ch 24–25), nonlinear optimization, MPC, SCP, collision avoidance and GCS, CBF and differentiable optimization, solvers), 26 widgets, 17 algorithm cards, 15 papers, 140-term glossary. |

## Known gaps and unverified items

- **Code not run:** CVXPY was not installed on the build machine, so the CVXPY snippets in Days 1, 3, 4, 5, 12, 17, 19 and 21 were not executed. Their expected outputs were checked by hand, with numpy, or against the in-page JS solvers. The NumPy and plain-Python snippets in Days 7, 8, 9 and 15 were run.
- **Sources read only in part:**
  - Açıkmeşe & Ploen 2007: abstract only. The entry relies on the abstract plus Blackmore 2010 and Malyuta 2022.
  - Kong 2015: abstract only.
  - GCS (Science Robotics 2023): numbers come from arXiv v1.
  - Safe flight corridors: the pipeline is described from the DecompUtil README and a 2024 third-party paper.
- **Statements taken from secondary sources:**
  - The SpaceX link rests only on Blackmore's 2017 NAE chapter (CVXGEN-generated flight code). Malyuta et al. only hypothesise the Falcon 9 connection.
  - Day 16's NLP theory (LICQ, second-order conditions, SQP) rests on standard sources and Wikipedia. Nocedal & Wright was not opened.
  - Day 15's history and speed-up claims (branch-and-cut in 1990, the 2.5×10¹¹ speed-up) are reported "as CO4A states".
- **Example values, not sourced:**
  - Malyuta's SCvx trust-region thresholds (0, 0.1, 0.7) are shown as example values; the PDF table would not extract cleanly.
  - The MPC parameters (u_max = 2 m/s², a 3.5 m lane, 25 m/s) are illustrative, not standards.
- **Claims without a source chip:**
  - IPOPT as a common back end for robot trajectory optimisation (Day 11).
  - The friction-circle SOCP example (Day 5).
  - Some recap-table rates and costs (Day 14).
- **Fast-moving facts (as of Oct 2026):** solver versions, GPU solver features (Gurobi PDHG on GPU, cuOpt, cuPDLP, OSQP 1.x CUDA), CVXPY's solver table counts (31 LP / 25 QP / 17 SOCP / 9 SDP, re-checked 2026-10-26), and the cvxpylayers 1.2.0 release (2026-05-19).
- **Testing limits:** the phone layout was checked at 360 px inside a headless iframe, and no page scrolls sideways. It has not yet been checked on a physical phone. Some review screenshots were taken with fallback fonts because Google Fonts requests stalled in headless Edge.
