# RUN-20261019T090402Z-kb-synthesizer — report

## What I was asked to do

Process the Stage 1 PR #2 input `inputs/LIN-GTB-3.md` (Linear issue GTB-3, "New
'My orders' screen in the app", state Ready) and its 8 exported Figma frames
into the PRD, in question mode `decide`.

## What I did

Read the issue in full (it is in English) and opened all 8 images in
`inputs/attachments/LIN-GTB-3/`. Commit `d0460b2` on
`notes/linear-GTB-3-20261018T171509Z` adds:

- `prd/requirements/FR-0001.md` to `FR-0005.md`
- `prd/modules/mobile.md` (new module narrative)
- `questions/Q-0001.md` to `Q-0003.md`
- `decisions.md`: DEC-0001 to DEC-0006
- `glossary.md`: 9 terms; `roles.md`: role `customer` and its mobile permissions

(The commit printed a warning that it could not lock `packed-refs` in the main
repo. `git status` shows the commit is on the branch and the tree is clean.)

## Per input: what was extracted and where it went

`inputs/LIN-GTB-3.md`:

| Line(s) | Statement | Went to |
|---|---|---|
| 12 | Design is done, Figma link | FR-0005 context; Q-0001 |
| 14 | Current list shows order number, item count, total | FR-0001 context; DEC-0001; Q-0003 |
| 17–18 | Card with date, status chip (placed amber, shipped green, cancelled grey), total | FR-0001.AC-1, AC-2; glossary |
| 19 | Grouped by month, sticky month header | FR-0001.AC-3, AC-4; DEC-0002 |
| 20–21 | Tapping a card opens the order with lines (product name, quantity, line total); the screen is new | FR-0001.AC-5; FR-0002 |
| 22–23 | Cancel moves to the order screen, bottom, text button; keep the confirmation dialog | FR-0003; DEC-0004; Q-0002 |
| 24–25 | Empty state uses the illustration from Figma's "Assets" page | FR-0004; Q-0001 |
| 27 | Spacing and type per Figma | FR-0005; Q-0001 |
| 27–28 | Dark mode in the file, can be a follow-up | DEC-0005; out of scope in FR-0001, FR-0002, FR-0004, FR-0005 |
| 30–31 | Detail uses `GET /api/v1/orders/{id}`, already in the generated client | FR-0002.AC-3; no API change |
| 1–9 | Front matter (state Ready, labels design/Mobile) | Module `mobile` |
| 33–44 | Globuli's Designs section: 8 frames, more not exported | See below |

**Figma frames.** None of the 8 frames shows My orders. `6186-2215-i.png` and
`7531-7432-i.png` are section title banners ("LOCAL COMPONENTS", "ACTIVE COLLAB
CARD"). `6186-2217-local-components.png` is a component sheet: People, Messages,
Navigation, Video calls. `7892-44286-…` and `7998-9098-active-collab-card.png`
show a "Today" calendar/meeting screen. The three `virtual-rooms` frames show a
"Campus" chat list. No order card, month header, status chip, order screen or
"Assets" illustration appears in any of them. None of the frames' content was
taken into requirements. This gap is Q-0001 (maintainer), and it blocks FR-0004
and FR-0005.

## Statements judged irrelevant

- Everything in the 8 frames, which belong to another product.
- The "Mobile-App" file name in the Figma URL: no product content.

## Decisions recorded (decide mode)

DEC-0001 (card content), DEC-0002 (sort, month grouping, locale), DEC-0003
(order screen header, amounts from the API, loading and error), DEC-0004
(Cancel for placed orders only), DEC-0005 (dark mode deferred), DEC-0006 (empty
state message). All are `kb-synthesizer (default)` with LIN-GTB-3 as their
source.

## Questions filed

- Q-0001: maintainer, high, blocks FR-0004 and FR-0005.
- Q-0002: client, medium, blocks nothing.
- Q-0003: client, low, blocks nothing.

## Statuses and priorities proposed

| Req | Status | Priority | Why |
|---|---|---|---|
| FR-0001 | ready | must | The core of the issue; fully testable from the text and DEC-0001/0002 |
| FR-0002 | ready | must | Cards open it; the client notes it doesn't exist today |
| FR-0003 | ready | must | Moving Cancel off the list removes today's only way to cancel, so the order screen must offer it in the same release |
| FR-0004 | clarifying | should | Needed for polish, but the app works without it; blocked by Q-0001 because the illustration is missing |
| FR-0005 | clarifying | should | Visual fidelity; no testable criterion can be written without the frames (Q-0001) |

I wrote the glossary code names (`order`, `orderLine`, `status`, `placed`,
`shipped`, `cancelled`, `myOrders`, `orderDetail`, `statusChip`) without seeing
the code repos. Please check them against the mobile app and orders-service in
review, since they can't be renamed once used.

## Change proposals

None. There were no requirements before this run.

## Proposed overview changes

- `prd/overview.md`, Users: "the app `demo-customer`". Consider stating there
  that each customer sees only their own orders, once login exists. Nothing to
  change now.

## What I couldn't do

- I couldn't see the My orders designs or the empty-state illustration, because
  they weren't among the exported frames. The Figma links don't open for me
  (Q-0001).
