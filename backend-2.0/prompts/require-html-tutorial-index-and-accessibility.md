# Require Every HTML Tutorial to Be Indexed and Accessible

## Feature context

`AGENTS.md` defines repository-wide rules for producing playbook artifacts. Its current `## Artifacts` section at lines 179–192 identifies detailed HTML lessons as an artifact, but it does not require a new HTML tutorial to be discoverable from the root landing page or verified for mobile and accessibility behavior.

Without a durable publishing rule, a future lesson can be created successfully but remain undiscoverable from GitHub Pages, difficult to use on a phone or iPad, or inaccessible to keyboard and assistive-technology users.

Add one repository-wide rule immediately after the existing Artifacts section. This is a documentation-only change; do not change any lesson HTML, the planned root `index.html`, or other project instructions in this task.

## Verified constraints and gotchas

- `AGENTS.md` is currently untracked and belongs to the user. Preserve all of its existing content exactly and insert only the approved block.
- The root `index.html` is planned in `backend-2.0/prompts/add-mobile-index-and-github-pages.md` but does not yet exist. The rule must handle both states: update it when present, or create it as part of the same tutorial change when absent.
- “Accessible” must be stated as concrete minimum checks rather than an unsupported claim of full standards conformance.
- A new tutorial and its index entry must be one atomic change so the published site cannot gain an orphaned lesson.
- Use relative links ending in the tutorial's `index.html` so links work both locally and under the GitHub Pages repository path.

## File 1: update `AGENTS.md`

Insert the new block after current line 192 and before current line 194 (`## Session Handoff`).

### Before — current lines 179–194

```md
## Artifacts

When a lesson reaches sufficient maturity, prepare as appropriate:

- detailed HTML lesson
- ADR when justified
- five-minute cheat sheet
- HealthSure update
- system/decision diagram
- Architect's Challenge
- open questions
- Architect's Reflection

Artifacts record reasoning actually developed during the lesson, not generic tutorial content.

## Session Handoff
```

### After — replacement beginning at line 179

```md
## Artifacts

When a lesson reaches sufficient maturity, prepare as appropriate:

- detailed HTML lesson
- ADR when justified
- five-minute cheat sheet
- HealthSure update
- system/decision diagram
- Architect's Challenge
- open questions
- Architect's Reflection

Artifacts record reasoning actually developed during the lesson, not generic tutorial content.

## HTML Tutorial Publishing Rule

Whenever a new HTML tutorial or lesson is added:

- add it to the repository-root `index.html` in the same change, using a descriptive title and a direct relative link to the tutorial's `index.html`;
- if the root `index.html` does not yet exist, create it as part of that same change rather than leaving the tutorial undiscoverable;
- include `<meta name="viewport" content="width=device-width, initial-scale=1">` and responsive styling that prevents horizontal page overflow at phone and tablet widths;
- use semantic HTML with a logical heading hierarchy, descriptive link text, readable contrast, visible keyboard focus, and keyboard-accessible navigation;
- keep core lesson content and navigation usable without JavaScript; and
- verify the root index and the new tutorial locally at phone, tablet, and desktop widths before considering the artifact complete.

An HTML tutorial is not complete if it is missing from the root index, has a broken link, or fails these minimum mobile and accessibility checks.

## Session Handoff
```

## Verification

Run from the repository root after implementation:

```sh
rg -n '^## HTML Tutorial Publishing Rule$|repository-root `index.html`|minimum mobile and accessibility checks' AGENTS.md
```

Manually verify:

- The new section appears once, immediately after `## Artifacts` and before `## Session Handoff`.
- All original `AGENTS.md` content remains present and unchanged outside the inserted block.
- The rule covers discoverability, direct relative HTML linking, mobile viewport/responsiveness, semantic structure, keyboard access, readable contrast, no-JavaScript core use, and local verification.
- The rule explicitly handles the case where the repository-root `index.html` does not yet exist.

## Acceptance criteria

- `AGENTS.md` contains one unambiguous HTML tutorial publishing rule.
- Every future HTML tutorial is required to update or create the root index in the same change.
- The rule defines concrete minimum mobile and accessibility checks.
- The rule makes a missing index entry, broken link, or failed minimum check a completion blocker.
- No other file is changed by this task.
