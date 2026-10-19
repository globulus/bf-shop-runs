# RUN-20261019T090401Z-kb-synthesizer — report

## Asked

Process the new input of Stage 1 PR #1, `inputs/LIN-GTB-2.md` (Linear issue
GTB-2, "Let customers add a delivery note to their order"), in `decide` mode.

## Done

Commit `f3f000a` on `notes/linear-GTB-2-20261018T171509Z`. The knowledge repo
was empty apart from templates, so the IDs start at 0001. The file is new on
this branch: main has no earlier version, so there's no diff to read. The
issue has no `designs`.

## Per input: LIN-GTB-2

| Statement (line) | Where it went |
|---|---|
| Support gets emails with delivery instructions after ordering (10–12) | Context of FR-0001; orders module narrative |
| Free-text note on the order, filled in when placing, seen by the warehouse (11–12) | FR-0001 user story and AC-1; warehouse part: DEC-0007, Q-0001 |
| Optional note in the web order form and the app's order screen (16–17) | FR-0001.AC-2, FR-0002.AC-1/AC-2, FR-0003.AC-1/AC-2 |
| Max 280 characters, because the label printer cuts off (17–18) | FR-0001.AC-3 + examples; FR-0002.AC-3, FR-0003.AC-3; DEC-0001, DEC-0003; Q-0002 |
| Stored with the order and returned when fetched (19) | FR-0001.AC-1, AC-5, AC-6 |
| Web order list shows a small note icon (20) | FR-0002.AC-4, DEC-0005 |
| "Mobile can show it on the order in My orders" (20–21) | FR-0003.AC-4, DEC-0006 (settled as: it does show it) |
| Can't be changed after placing; email support (22–23) | FR-0001.AC-4, FR-0002.AC-5, FR-0003.AC-5, DEC-0004; out of scope in each |
| Out of scope: note in the confirmation email (25–26) | Out of scope in FR-0001/2/3 |
| Backend first, then web and mobile in parallel (28) | Split into FR-0001 (orders) with FR-0002/FR-0003 `depends_on: [FR-0001]` |
| Labels `Feature, orders-service, web, Mobile` (front matter) | Matches the three modules |

Also updated: glossary (delivery note `deliveryNote`, My orders), `roles.md`
(role `customer` and its permissions per module), module narratives
`prd/modules/orders.md`, `web.md` and `mobile.md`.

## Judged irrelevant

- "notifications aren't wired up yet anyway" (line 25–26): background to the
  out-of-scope line, no requirement.
- Front matter `state: Ready`, `updated`, `url`: intake metadata.
- Support and the warehouse are mentioned but have no screens in the product;
  not added as roles (the warehouse part is in DEC-0007/Q-0001).

## Decisions recorded

DEC-0001 to DEC-0007 (all `kb-synthesizer (default)`, source LIN-GTB-2):
- DEC-0001: how the limit is counted and what happens above it (Q-0002 stays open)
- DEC-0002: trimming, empty notes, null for no note, older orders
- DEC-0003: field limit in the forms, no counter
- DEC-0004: what "can't be changed" means for the API and the UIs
- DEC-0005: web list icon (accessible label, no note text)
- DEC-0006: app shows the note in My orders
- DEC-0007: warehouse sees the note through the API only (Q-0001 stays open)

## Questions filed

- Q-0001 (client, `blocks: []`, medium)
- Q-0002 (client, `blocks: []`, low)

No maintainer questions; nothing blocks. I left `open_questions` empty in the
requirements' front matter because neither question blocks. FR-0001's
front-matter comment and context name both questions.

## Priorities proposed

FR-0001, FR-0002, FR-0003: `must`. The issue lists all three parts under
"What we need" and is in state Ready. FR-0001 comes first because the other
two depend on it. FR-0003 is `must` too: the order screen field is required
("in both"). Only the My orders display was optional ("can show"), and
DEC-0006 includes it.

## Statuses

All three requirements are set `ready`. Each has a user story, a source, testable
Given/When/Then criteria (5–6 each), an examples table where there's a rule to
check (FR-0001), out-of-scope lines and no blocking questions. The maintainer
approves the status by merging this PR. I didn't have Globuli's
`docs/requirement-lifecycle.md` in this workspace, so I checked against the
template and my role constitution, not the lifecycle document itself.

## Change proposals

None (no existing requirements).

## Proposed overview changes

None needed. Optionally, the Users section could say that the warehouse and
support read orders outside the product (pending Q-0001).

## Not done

Nothing left undone.
