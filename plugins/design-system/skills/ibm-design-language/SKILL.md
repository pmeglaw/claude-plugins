---
name: ibm-design-language
description: Design and build interfaces in the IBM Design Language and Carbon Design System — exact color/type/spacing/motion tokens, the 2x Grid, UI shell and global header, the seventeen universal patterns, status indicators, and accessibility floors. Use this whenever the work touches IBM, Carbon, @carbon/react, IBM Plex, "Carbon tokens", a UI shell or global header, or an enterprise/admin/product UI that should look like IBM — and also when someone asks a question this system already answers, such as "modal or toast?", "should this be disabled or read-only?", "what color for this status?", "how much space between these?", or "is this contrast good enough?", even if they never say the words IBM or Carbon.
---

<!-- source: https://www.ibm.com/design/language/whats-new · @carbon/react 1.114.0 · checked 2026-08-24 · spacing scale corrected against @carbon/layout 11.57.0 generated source 2026-08-26 -->

# IBM Design Language & Carbon

## The thesis

IBM's stated purpose for design is **to guide** — to move someone from here to there with the least friction, and to leave them with time saved or time well spent. Carbon v12 sharpens that into a working rule: **complexity waits until it is useful.** Reveal depth in sequence; surface the right action at the right moment; let hierarchy, contrast and motion do that work together.

Almost every specific rule below is a consequence of one of those two sentences. When a case isn't covered here, reason from them rather than guessing.

## How to work

1. **Read the request for what kind of surface it is.** A tool that gets operated (dashboard, admin, planner, table) is *productive*: fixed type set, gray-dominant surfaces, dense spacing, motion in tens of milliseconds. A page that gets read (marketing, landing, editorial) is *expressive*: fluid type, larger scale, more air. Mixing the two is the most common way IBM work goes wrong.
2. **Set up a token layer before writing any component CSS.** `${CLAUDE_PLUGIN_ROOT}/skills/ibm-design-language/assets/carbon-tokens.css` is a drop-in for both themes; copy it in rather than retyping hexes.
3. **Build against the decision tables below.** Most design questions in this system have an answer already; look before inventing.
4. **Check contrast once, as a batch, with `python ${CLAUDE_PLUGIN_ROOT}/skills/ibm-design-language/scripts/check_contrast.py --preset all`** (needs Python on PATH; skip it and use the traps table below if Python is unavailable) — not colour by colour. Several of Carbon's own status tokens fail as drawn marks in light themes, and the failures cluster on the *hover* surface, which is easy to miss when checking pairs one at a time. `${CLAUDE_PLUGIN_ROOT}/skills/ibm-design-language/references/tokens.md` lists the known traps so you can design around them before you write any CSS.
5. **Run the review checklist at the end.**

A note on effort: this system rewards getting the constraints right up front, not iterating toward them. Read the one reference file the task actually needs, take the traps table as given rather than rediscovering it, and verify once at the end. A build that reads every reference and re-runs the checker after each colour change costs a great deal and lands in the same place.

Load a reference file when you're actually in that territory — they're detailed and there's no value in reading all of them. Paths below use `${CLAUDE_PLUGIN_ROOT}`, which resolves to this plugin's install directory; if it ever comes through unsubstituted, the files sit in `skills/ibm-design-language/` beside this one.

| File | Read it when |
|---|---|
| `${CLAUDE_PLUGIN_ROOT}/skills/ibm-design-language/references/tokens.md` | Picking any color, type, spacing, motion or breakpoint value; setting up theming |
| `${CLAUDE_PLUGIN_ROOT}/skills/ibm-design-language/references/patterns.md` | Choosing between dialog/notification variants, forms, empty states, search, filtering, loading, disabled vs read-only |
| `${CLAUDE_PLUGIN_ROOT}/skills/ibm-design-language/references/ui-shell.md` | Building a header, side nav, right panel, breadcrumb, or deciding product-vs-system scope |
| `${CLAUDE_PLUGIN_ROOT}/skills/ibm-design-language/references/status-and-dataviz.md` | Designing status indicators, seat/node/device states, charts, or any categorical color |
| `${CLAUDE_PLUGIN_ROOT}/skills/ibm-design-language/references/carbon-next.md` | Questions about v12, feature flags, DTCG token renaming, Carbon for AI, or future-proofing |

## Non-negotiables

These are the ones that get missed, and each one is visible at a glance to anyone who knows the system:

- **8px mini unit, Carbon spacing scale.** Spacing values come from Carbon's token scale — **2, 4, 8, 12, 16, 24, 32, 40, 48, 64, 80, 96, 160px** (spacing-01–13) — not from plain 8-multiples: 2/4 (micro), 12 and 40 are real Carbon steps, so never flag them as off-grid. The 1x/2x/3x/4x/6x/8x/10x/12x mini-unit multiples are the separate *layout sizing* rule, not the spacing set. Element heights come from a fixed ladder — 24, 32, 40, 48, 64, 80px — never from padding math; the one sanctioned off-ladder height is the 44px touch target (below).
- **Zero border radius.** Carbon UI is square. The only rounded thing is a tag (16px) and a badge dot.
- **IBM Plex**, flush left, sentence case. Never all-caps paragraphs. Never two emphasis devices on the same words ("belt and suspenders").
- **Blue 60 `#0f62fe` is the only primary action color** across every IBM product. Other hues are used sparingly and for meaning, not decoration.
- **Focus is 2px, `$focus`, inset** (`outline: 2px solid; outline-offset: -2px`). Never removed, never rounded, never a glow.
- **Status needs two signals minimum** — color plus shape or symbol. Color alone never carries meaning.
- **Touch targets 44px — and 44 beats the ladder.** A 16px icon gets padding to reach it; the icon does not grow. 44 sits on neither the height ladder nor the spacing scale; the touch minimum is a WCAG requirement while the ladder is an aesthetic convention, so when they conflict the touch minimum wins. A 44px control is correct, not drift.
- **Grays dominate.** If a screen reads as colorful, something has gone wrong.

## Decision tables

### How much may I interrupt?

Rank by consequence and default low. The system may never interrupt on its own initiative — a dialog must follow from a user action, and a background process finishing is *not* a user action.

| Reach for | When |
|---|---|
| **Callout** | Guidance the user should read *before* acting. Loads with the page, never dismissible, never triggered. No success/error status exists for it. |
| **Inline notification** | Task-generated feedback, placed in the region the user is working in. Under two lines. This is the default. |
| **Toast** | System-generated message with no place on the page. Fixed width, ≤3 lines, newest on top. With an action, it must persist until dismissed. |
| **Banner** | Product- or system-wide message unrelated to any task. One at a time, scrolls with content. |
| **Dialog** | Short, focused, user-initiated task; or a decision you must confirm. |
| **Danger modal** | Irreversible consequence. High-impact destruction also requires typing the resource name. |

Two hard stops: **never nest modals** (if a modal task needs a confirmation modal, that task shouldn't be in a modal), and **never put large or complex data in a dialog** — that's a page.

### Where does this form go?

| Container | When |
|---|---|
| Dedicated page | Complex, long, or multi-step input |
| Dialog | Fewer than five inputs, infrequent, editing/management |
| Side panel | More than five inputs, or the user must keep referencing what's behind it |

Mark the minority: if most fields are required, mark only the optional ones, and vice versa. Buttons go at the bottom — never pinned to the top. Name them for the action ("Publish", "Assign seat"), never "Submit" or "OK".

### Disabled, read-only, or hidden?

This one is load-bearing because disabled components **are not read by screen readers and do not pass contrast**.

| State | When | Consequence |
|---|---|---|
| Disabled | Temporarily unavailable pending a user action or unmet dependency | Not reachable by keyboard, not announced. Only safe when the content doesn't need reading. |
| Read-only | The content still needs to be read — a process, a lock, or permissions prevent editing | Stays keyboard-navigable but not operable. Text keeps its color and still passes 4.5:1. |
| Hidden | The user lacks permission to know it exists | Absent entirely until permissions change. |

Never convert a disabled component to read-only just because the surrounding view became read-only.

### Which search?

| Type | When |
|---|---|
| Basic | Routes to a distinct results page. Large or expensive data sets. |
| Active | Small data set; filters in place as the user types; no results page. |
| Focused | Actively searches the current scope, with an option to widen to everything. |

Never label a search field. **Always publish the number of results, zero included** — silence is a dead end.

### Filtering

Instant updates for a single category or small data; batch updates ("Apply") when selections span categories or the data is slow. **Multiple filter categories must never live inside a menu or dropdown** — put them in a left rail or a top strip. A collapsed filter needs a visible applied-count and a way to clear without reopening. Every category needs a clear-all; multiple categories need a global clear-all.

### Destructive actions

| Impact | Treatment |
|---|---|
| Low — trivially undone | Just do it |
| Moderate — can't be undone easily, or affects several things | Confirm with the consequences spelled out |
| High — expensive to recreate, or cascades into other objects | Confirm **and** make the user type the resource name |

Afterward: return to the list, animate the row out, confirm with a notification. If it fails, say so — and on a second channel if you can.

## Motion budget

Two families. **Productive** is how an interface answers you; **expressive** marks beginnings and arrivals. Elements move on one axis at a time with slight overlap. Nothing bounces, overshoots, or settles on a long exponential ease.

| Duration | ms | Use |
|---|---|---|
| fast-01 | 70 | Buttons, toggles, seat hover |
| fast-02 | 110 | Fades, hover color, nav expand |
| moderate-01 | 150 | Small expansion, short distance |
| moderate-02 | 240 | Panels, toasts, modals |
| slow-01 | 400 | Large expansion, important alerts |
| slow-02 | 700 | Background dimming |

Productive easing: standard `cubic-bezier(.2,0,.38,.9)`, entrance `cubic-bezier(0,0,.38,.9)`, exit `cubic-bezier(.2,0,1,.9)`.
Expressive easing: standard `cubic-bezier(.4,.14,.3,1)`, entrance `cubic-bezier(0,0,.3,1)`, exit `cubic-bezier(.4,.14,1,1)`.

Duration scales with distance and size. Always honor `prefers-reduced-motion`.

## Token discipline

Product code should never reference a raw hex, and ideally never a Carbon token directly either. Keep one semantic layer in between:

```css
--seat-assigned-bg: var(--cds-layer-selected-01);   /* product meaning → system token */
```

Two reasons this earns its keep. It makes intent readable in review — `--seat-conflict-border` says what a hex can't. And Carbon v12 is migrating token naming to the DTCG spec; when names change, a semantic layer means one file changes instead of every component. As of `@carbon/react` 1.114.0 that migration has real tooling (a v12 codemod in `@carbon/upgrade`) — and a codemod operates on this layer, so keeping it clean is the concrete v12 preparation. See `references/carbon-next.md`.

## Type

Product UI uses the **fixed** set — sizes never change with breakpoint, the container does. The whole scale derives from `Xn = Xn-1 + {INT[(n-2)/4] + 1} × 2` starting at 12px.

| Token | Size/leading | Weight | Use |
|---|---|---|---|
| `label-01` | 12/16 | 400 | Field labels, captions, helper text |
| `body-compact-01` | 14/18 | 400 | Table rows, dense UI |
| `body-01` | 14/20 | 400 | Paragraphs in product |
| `heading-compact-01` | 14/18 | 600 | Column headers, tile titles |
| `heading-03` | 20/28 | 400 | Section and modal headings |
| `heading-04` | 28/36 | 400 | Page headings |
| `heading-06` | 42/50 | **300** | Display — note the Light weight |

Large display sizes get *lighter*, not bolder. That inversion is a signature of the system.

For CJK, Thai, Devanagari and Arabic, reduce size to 95% and keep the line height.

## Accessibility floor

- 4.5:1 for text under 24px, 3:1 for large text and for graphical elements including status indicators and chart marks.
- **Check a mark against the surface it lands on when hovered, not at rest.** Light-theme rows and tiles lighten on hover, so `layer-hover-01` (#e8e8e8) is the worst case, not white. Carbon's own green 50 passes on white and fails on a hovered row.
- The palette is a uniform 12-grade ladder, so contrast becomes counting steps. Full table in `references/tokens.md`; roughly, a grade-60 color needs 4 steps of separation for 4.5:1.
- Against a gradient, check text against the **lowest-contrast stop**, not the one it currently sits over — text moves when users resize or respace it.
- Keyboard: a spatial grid (map, canvas, seating chart) uses **roving tabindex with arrow keys**, not one tab stop per cell. Escape clears. A "Skip to main content" link is the first focusable element on the page.
- Landmark regions for every major area; unique labels when there's more than one `navigation`.
- Match DOM order to visual order (WCAG technique C27).

## Review checklist

Run this before calling anything finished:

- [ ] No raw hex or arbitrary px outside the token layer
- [ ] Every spacing value is on the Carbon spacing scale (2/4/8/12/16/24/32/40/48/64/80/96/160); every control height is on the ladder or is a 44px touch target
- [ ] Radius is 0 everywhere except tags
- [ ] Focus visible on every interactive element, 2px inset, not removed anywhere
- [ ] Every status carries two signals; no state distinguished by color alone
- [ ] Contrast checked with the script, including status borders and chart marks
- [ ] Both themes rendered and read — light and dark, not just one inverted
- [ ] Keyboard path complete: skip link, landmarks, arrow keys where a grid exists, Escape closes
- [ ] Every empty state and error names a next step
- [ ] Every search reports a result count, including zero
- [ ] Motion durations from the table, one axis at a time, reduced-motion honored
- [ ] Nothing interrupts that the user didn't initiate

## A caution about v12

Carbon v12 is published as a direction, not a shipped spec — no dates, no visual specimens. It arrives incrementally through `enable-v12-*` feature flags, all defaulting off, while v11 stays active. **Build v11 properly and opt into flags as they prove out.** Designing against an undated roadmap is how products end up half-migrated to something that never shipped. See `references/carbon-next.md` for what is actually shipping today.

## Is this actually new?

Footer "last updated" dates on carbondesignsystem.com and ibm.com/design/language are static-site BUILD stamps. They move on every deploy and are not evidence of a content change. Never start a docs refresh from one.

Authoritative checks instead:

- IDL → ibm.com/design/language/whats-new (the announcement page)
- Carbon → `npm view @carbon/react time --json` (real publish dates) + the changelog for any version that moved

carbondesignsystem.com is JS-rendered and unreliable for exact values. For any Carbon number — spacing, type, motion, colour — read the canonical npm package source (`@carbon/layout`, `@carbon/type`, `@carbon/motion`, `@carbon/colors`), not the docs site.

Confirmed build-stamp-only false alarms: 2026-08-21 and 2026-08-24.
