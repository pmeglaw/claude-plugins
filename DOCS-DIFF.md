# DOCS-DIFF — @carbon/react 1.112.0 → 1.114.0 vs ibm-design-language skill

Checked 2026-08-24. Sources: GitHub releases v11.112.0 (2026-07-15), v11.113.0 (2026-07-30), v11.114.0 (2026-08-12); `packages/feature-flags/feature-flags.yml` diffed v11.111.0 → v11.114.0; PRs #22215, #22617, #22867, #22978 read in full. (Monorepo tags are `v11.x`; `@carbon/react` inside them is `1.x`.)

Headline: **zero contradictions.** No token renamed/removed, no default value changed, no flag default flipped, no deprecation inside skill scope. Everything below is "same" or additive.

## Feature flags — the priority check

| Claim | Skill says | Verdict |
|---|---|---|
| All `enable-v12-*` flags `enabled: false` at 1.114.0 (feature-flags.yml, v11.114.0 tag) | "all defaulting off" | **same** — no flip. `enable-v11-release` remains the only `true`. |
| `enable-v12-release` flag first CREATED in 1.112.0 (#22619 "set up v12 code path delivery mechanisms"), off | Skill's flag table already lists it | **same** (list accurate; the flag is newer than the skill implies — cosmetic) |
| yml also holds flags the skill's table omits: `enable-treeview-controllable`, `enable-enhanced-file-uploader`, `enable-experimental-tile-contrast`, `enable-experimental-focus-wrap-without-sentinels`, legacy `enable-css-*`, `enable-v11-release` | Table scoped to v12-relevant flags | **absent** — deliberate scope, no rule change needed |

## Component / token claims

| Claim | Skill says | Verdict |
|---|---|---|
| `Card` migrated from `@carbon/ibm-products` into `@carbon/react` — composable `Card.Header/Title/Body/Footer`, `density`, AI-label decorator (1.114.0, #22867) | carbon-next.md: "Carbon for IBM Products … opening to the community"; "don't hand-build what's about to be given away" | **new** — first concrete migration landing in core; strengthens existing advice. One line in carbon-next.md. |
| v12 tooling shipped: `@carbon/upgrade` v12 release codemod (#22784), `v12.md` migration guide (#22884), labs v12 DatePicker → Preview (all 1.114.0) | "v12 arrives incrementally via flags" | **new** — mechanism detail, direction unchanged. One line. |
| DTCG artifacts shipped: themes "DTCG Explorations" (1.113.0 #22326), motion tokens in DTCG format (1.114.0 #22743), Style Dictionary v5 pipeline (1.114.0) | "DTCG adoption in progress, backward compatible, zero consumer impact" | **same** — claim confirmed, now with shipped evidence; can cite versions |
| IconIndicator/ShapeIndicator gain `compact` mode + `align`/`autoAlign`/`iconDescription` props (1.112.0 #22215) — **Preview-tier** components | status-and-dataviz.md: icon indicator "Requires icon, shape, meaningful color, and a descriptive inline label" | **new, preview-only** — Carbon now sanctions a label-less compact variant (accessible name via description/tooltip). Closest thing to a contradiction in the pass, but Preview tier = direction, not doctrine. Flag as note; do NOT rewrite the label rule. |
| Preview/unstable Pagination DEPRECATED; stable Pagination gains `renderPageSelect` (1.113.0 #22617) | Skill silent on pagination | **absent** — out of skill scope, nothing to change |
| Popover now closes on Esc (1.113.0 #22649) | Checklist: "Escape closes" | **same** — Carbon caught up to the rule the skill already states |
| Modal keeps default `dialog` role on non-alert modals (1.112.0 #22668) | No role-level claim in patterns.md | **same/absent** — consistent with existing dialog-semantics guidance |
| `$button-min-inline-size` exposed as public Sass variable, default unchanged at 11rem/176px (1.114.0 #22978, explicitly non-breaking) | No claim | **absent** — Sass-config detail below doctrine level |
| Initial motion JS API in `@carbon/motion` (1.113.0 #22706); duration/easing token VALUES untouched | Motion table (70/110/150/240/400/700ms + beziers) | **same** for all values; API itself **new** — first shipped artifact of the v12 "motion as structure" shift. One line in carbon-next.md at most. |
| Hover styles guarded with `any-hover` across many components (1.113–1.114) | No touch-hover rule | **absent** — implementation detail |
| OverflowMenu danger-option contrast fix (1.112.0 #22566) | Contrast traps table (status tokens on hover surfaces) | **same** — different surface, doesn't touch the trap table |

## Verified negative — baseline for the next refresh

Verified 2026-08-24 against @carbon/react 1.112.0–1.114.0 (release bodies + PRs #22215, #22617, #22867, #22978), feature-flags.yml diffed v11.111.0 → v11.114.0:

- zero token renames, removals, or default value changes in skill scope
- zero deprecations in skill scope
- every `enable-v12-*` flag still `enabled: false`; `enable-v12-release` added in 1.112.0, also off
- motion token values unchanged
- contrast trap table unchanged

Method for repeating this: diff `packages/feature-flags/feature-flags.yml` between monorepo tags; read release bodies for the intervening minors only. Referenced from `references/carbon-next.md` so the next refresh starts here.

Provenance data point (not a skill edit): the 1.113.0 popover-closes-on-Esc fix (#22649) means Carbon implemented a rule the skill already stated — the skill was ahead of the implementation in that instance.

## RC

`@carbon/react` 1.115.0-rc.0 published 2026-08-24 (tag v11.115.0-rc.0; GitHub release body is empty — nothing to read). Per policy: note in carbon-next.md as **anticipated**, version + date only. Nothing from it becomes doctrine.

## Proposed edits (pending approval)

1. `references/carbon-next.md` — add to "Declared versus shipped": Card migration (1.114.0), v12 codemod + migration guide (1.114.0), DTCG shipped artifacts (1.113–1.114), anticipated 1.115.0-rc.0 line. Add source comment.
2. `references/status-and-dataviz.md` — one-line note: Preview-tier compact indicator variant exists (1.112.0); inline-label rule stands for stable components. Add source comment.
3. `SKILL.md` — "Is this actually new?" section (already applied, uncommitted) + source comment.
4. No changes to tokens.md, patterns.md, ui-shell.md, assets — nothing moved.
5. Version bump patch in both `marketplace.json` and `plugin.json`.
