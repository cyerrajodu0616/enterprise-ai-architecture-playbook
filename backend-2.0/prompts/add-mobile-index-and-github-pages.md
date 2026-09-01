# Add a Mobile-Friendly Lesson Index and Publish with GitHub Pages

## Feature context

The repository currently contains three standalone HTML lessons:

- `bootcamp/week01-knowledge-systems/day01-knowledge-access/index.html`
- `bootcamp/week01-knowledge-systems/day02-long-context-vs-retrieval/index.html`
- `bootcamp/week01-knowledge-systems/day03-document-ingestion-structure-versions-metadata/index.html`

Each page already includes a viewport meta tag. Days 2 and 3 also include explicit mobile breakpoints. There is no `index.html` at the repository root, so GitHub Pages has no purpose-built landing page that lets a phone or tablet user discover the available lessons.

The simplest solution is a dependency-free root landing page and GitHub Pages configured to publish the repository root from the durable default branch. Do not introduce a JavaScript framework, package manager, generated site, custom domain, or GitHub Actions workflow for this scope.

## Verified constraints and gotchas

- Preserve all existing lesson HTML files unchanged.
- Preserve the existing untracked files shown by `git status`; they belong to the user.
- Use relative links so the page works both on GitHub Pages at `/enterprise-ai-architecture-playbook/` and when opened locally.
- Link directly to each lesson's `index.html`; do not link to its `README.md`.
- Keep the page fully usable at narrow phone widths, tablet widths, and desktop widths.
- Use semantic HTML, visible keyboard focus, sufficient contrast, and a reduced-motion preference.
- GitHub Pages configuration is repository state, not a code file in this design. After this change reaches the default branch, configure Pages to deploy from that branch and the `/ (root)` folder.
- The current checked-out branch is `agent/week01-day03-document-ingestion`; do not configure Pages to publish from this temporary working branch.

## File 1: create `index.html`

This is a new file, so there is no before block.

Create `index.html` beginning at line 1 with exactly:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="description" content="Enterprise AI Architecture Playbook lessons and architecture decision resources.">
  <title>Enterprise AI Architecture Playbook</title>
  <style>
    :root {
      color-scheme: light;
      --ink: #162033;
      --muted: #526176;
      --line: #d8e0ec;
      --blue: #174ea6;
      --blue-dark: #103b7d;
      --soft: #eef5ff;
      --paper: #ffffff;
      --page: #f3f6fa;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      background: var(--page);
      color: var(--ink);
      font: 16px/1.6 -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    a { color: var(--blue); }

    a:focus-visible {
      outline: 3px solid #f5a623;
      outline-offset: 4px;
    }

    .shell {
      width: min(100% - 32px, 1080px);
      margin: 32px auto;
    }

    .hero,
    .lesson-card {
      background: var(--paper);
      border: 1px solid var(--line);
      border-radius: 18px;
      box-shadow: 0 10px 35px rgba(22, 32, 51, 0.08);
    }

    .hero { padding: clamp(28px, 6vw, 64px); }

    .eyebrow {
      margin: 0 0 10px;
      color: var(--blue);
      font-size: 0.82rem;
      font-weight: 750;
      letter-spacing: 0.09em;
      text-transform: uppercase;
    }

    h1 {
      max-width: 18ch;
      margin: 0;
      color: #112b52;
      font-size: clamp(2.1rem, 7vw, 4.4rem);
      line-height: 1.05;
      letter-spacing: -0.035em;
    }

    .intro {
      max-width: 68ch;
      margin: 22px 0 0;
      color: var(--muted);
      font-size: clamp(1rem, 2.4vw, 1.2rem);
    }

    .principle {
      margin: 28px 0 0;
      padding: 16px 18px;
      background: var(--soft);
      border-left: 5px solid var(--blue);
      border-radius: 0 10px 10px 0;
      font-weight: 650;
    }

    .lessons { padding: 42px 0 8px; }

    .section-heading {
      display: flex;
      align-items: end;
      justify-content: space-between;
      gap: 16px;
      margin-bottom: 18px;
    }

    h2 {
      margin: 0;
      color: #112b52;
      font-size: clamp(1.55rem, 4vw, 2.15rem);
      line-height: 1.2;
    }

    .section-heading p {
      margin: 0;
      color: var(--muted);
    }

    .lesson-grid {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 18px;
    }

    .lesson-card {
      display: flex;
      flex-direction: column;
      min-height: 100%;
      padding: 24px;
    }

    .lesson-number {
      margin: 0 0 12px;
      color: var(--blue);
      font-size: 0.82rem;
      font-weight: 750;
      letter-spacing: 0.06em;
      text-transform: uppercase;
    }

    .lesson-card h3 {
      margin: 0;
      color: #112b52;
      font-size: 1.2rem;
      line-height: 1.3;
    }

    .lesson-card p {
      margin: 14px 0 20px;
      color: var(--muted);
    }

    .lesson-card a {
      align-self: flex-start;
      margin-top: auto;
      padding: 11px 15px;
      background: var(--blue);
      border-radius: 9px;
      color: #ffffff;
      font-weight: 700;
      text-decoration: none;
    }

    .lesson-card a:hover { background: var(--blue-dark); }

    footer {
      padding: 30px 0 10px;
      color: var(--muted);
      text-align: center;
    }

    @media (max-width: 820px) {
      .lesson-grid { grid-template-columns: 1fr; }
      .lesson-card { min-height: auto; }
    }

    @media (max-width: 560px) {
      .shell {
        width: 100%;
        margin: 0;
      }

      .hero {
        border-width: 0 0 1px;
        border-radius: 0;
        box-shadow: none;
      }

      .lessons,
      footer { padding-right: 20px; padding-left: 20px; }

      .section-heading {
        align-items: start;
        flex-direction: column;
      }
    }

    @media (prefers-reduced-motion: reduce) {
      * { scroll-behavior: auto !important; }
    }
  </style>
</head>
<body>
  <main class="shell">
    <header class="hero">
      <p class="eyebrow">Enterprise AI Architecture</p>
      <h1>Build judgment, not just systems.</h1>
      <p class="intro">A practical playbook for making, challenging, defending, and evolving enterprise AI architecture decisions based on business outcomes, evidence, cost, risk, governance, and operations.</p>
      <p class="principle">What is the simplest solution that satisfies the business requirement?</p>
    </header>

    <section class="lessons" aria-labelledby="lessons-heading">
      <div class="section-heading">
        <h2 id="lessons-heading">Week 1 lessons</h2>
        <p>Knowledge access and grounded answers</p>
      </div>

      <div class="lesson-grid">
        <article class="lesson-card">
          <p class="lesson-number">Day 1</p>
          <h3>How Should an AI System Access Enterprise Knowledge?</h3>
          <p>Start with authority, business questions, and the simplest access mechanism—not a predetermined retrieval pattern.</p>
          <a href="bootcamp/week01-knowledge-systems/day01-knowledge-access/index.html">Open Day 1</a>
        </article>

        <article class="lesson-card">
          <p class="lesson-number">Day 2</p>
          <h3>Long Context vs Retrieval</h3>
          <p>Decide when retrieval creates enough measurable value to justify another subsystem and its operating obligations.</p>
          <a href="bootcamp/week01-knowledge-systems/day02-long-context-vs-retrieval/index.html">Open Day 2</a>
        </article>

        <article class="lesson-card">
          <p class="lesson-number">Day 3</p>
          <h3>Document Ingestion, Structure, Versions &amp; Metadata</h3>
          <p>Treat evidence identity, authority, structure, applicability, authorization, and reproducibility as architectural concerns.</p>
          <a href="bootcamp/week01-knowledge-systems/day03-document-ingestion-structure-versions-metadata/index.html">Open Day 3</a>
        </article>
      </div>
    </section>

    <footer>
      <p>Architectural decisions are temporary. Architectural reasoning is timeless.</p>
    </footer>
  </main>
</body>
</html>
```

## GitHub Pages configuration (after merge)

After the new file is committed to the repository's durable default branch:

1. Open the GitHub repository.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, select **Deploy from a branch**.
4. Select the durable default branch and the **/ (root)** folder.
5. Save and wait for GitHub to report the published site URL.
6. Verify the root landing page and all three lesson links on a phone or tablet.

Do not guess the default branch name from the currently checked-out feature branch. Confirm it in GitHub before selecting it.

## Verification

Run these checks from the repository root:

```sh
test -f index.html
rg -n 'viewport|day01-knowledge-access/index.html|day02-long-context-vs-retrieval/index.html|day03-document-ingestion-structure-versions-metadata/index.html' index.html
```

Start a local static server:

```sh
python3 -m http.server 8000
```

Then verify:

- `http://localhost:8000/` loads without console errors.
- All three lesson buttons return HTTP 200 and open the intended lesson.
- At 390 px viewport width, content does not overflow horizontally and buttons are easy to tap.
- At 768 px viewport width, content remains readable and the lesson list has comfortable spacing.
- Keyboard Tab focus is clearly visible on all links.
- Disabling JavaScript does not affect the page.

## Acceptance criteria

- The repository root has a valid, semantic `index.html`.
- The root page links to every HTML lesson currently present in the repository.
- The page is usable on mobile, iPad/tablet, and desktop.
- No existing lesson or Markdown artifact changes.
- No new runtime dependency, build pipeline, custom domain, or JavaScript is introduced.
- GitHub Pages publishes from the durable default branch's repository root.
