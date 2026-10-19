# kb-synthesizer report — RUN-20261019T091104Z-kb-synthesizer

## What I was asked to do

Fix run (iteration 2) on Stage 1 PR #1, branch
`notes/linear-GTB-2-20261018T171509Z`: address the two kb-critic findings
from RUN-20261019T090649Z-kb-critic on the delivery-note requirements
FR-0001, FR-0002 and FR-0003 (source LIN-GTB-2). Question mode `decide`.

## What I did

Re-read `inputs/LIN-GTB-2.md`, the three requirements and `decisions.md`,
then amended them in one commit on the PR branch. No file in `inputs/` was
touched; no existing decision row was changed other than appending an
`*Amended by DEC-0008.*` marker to DEC-0001 and DEC-0003.

### Finding F-01 (field limit and server limit count differently; no criterion for a rejected note) — fixed

The issue says only "Max 280 characters". The critic is right that
DEC-0001 (server: code points after trimming) and DEC-0003 (clients: "280
characters", unit undefined) could be read differently, and that no
requirement said what the web shop or the app does when orders-service
still answers HTTP 400.

- `decisions.md`: added **DEC-0008** — the web shop and the app count the
  same unit as orders-service (Unicode code points after trimming), a
  character made of several code points counts as several at both ends, and
  CR LF is normalised to one LF before trimming and counting, by the
  clients in the field and by orders-service on receipt. Marked as amending
  DEC-0001 and DEC-0003; source LIN-GTB-2 (Q-0002 still asks the client to
  confirm the counting rule).
- `decisions.md`: added **DEC-0009** — when orders-service rejects the
  order because of the note, the clients place no order, show the
  validation error at the note field (localized) and keep everything the
  customer entered.
- `prd/requirements/FR-0001.md`: AC-3 names DEC-0008; context lists it;
  examples gain a 56-emoji row (280 code points, accepted), a 57-emoji row
  (285, HTTP 400) and a CR LF row (stored as one LF).
- `prd/requirements/FR-0002.md` and `FR-0003.md`: AC-3 names the counting
  unit (code points, not grapheme clusters); new **AC-6** for an order
  orders-service rejects because of its note (DEC-0009); context updated.

Why a decision rather than a question: the issue gives no unit, the
server-side rule was already decided in DEC-0001, and one shared unit is
the simplest choice that keeps the client able to change the rule later.
The CR LF normalisation is the usual way to make "a line break counts as
one character" (DEC-0001) hold for input from Windows browsers.

### Finding F-02 (AC-1 "alone or in the order list" lets the list omit the note) — fixed

- `prd/requirements/FR-0001.md`: AC-1 now reads "both when the order is
  fetched alone and when it appears in the customer's order list; each
  order in the list carries its own `deliveryNote`". Added an example row
  for a fetched order list with one order that has a note and one without.

## Change proposals

All three requirements are `ready` (set in iteration 1 of this PR, not yet
merged), so each amendment is recorded as a change proposal with a
`## Change history` entry dated 2026-10-19:

- FR-0001: AC-1 reworded (list response), AC-3 names DEC-0008, three new
  example rows.
- FR-0002: AC-3 names DEC-0008, new AC-6 (DEC-0009).
- FR-0003: AC-3 names DEC-0008, new AC-6 (DEC-0009).

No work items exist yet for these requirements, so no work is paused.

## Decisions recorded

- DEC-0008 (counting unit shared by both ends; CR LF as one character).
- DEC-0009 (client behaviour on a note orders-service rejects).

## Questions filed

None. Q-0002 (already open, client) covers confirmation of the counting
rule, which DEC-0008 extends; no new question is needed.

## Statements judged irrelevant

None new; this run only addressed the findings.

## Priorities

Unchanged (all `must`, as proposed in iteration 1).

## Proposed overview changes

None.

## What I could not do

Nothing outstanding. I did not change DEC-0001's or DEC-0003's text beyond
the appended marker, as the role constitution forbids editing a recorded
decision.

## Commits

- `docs(prd): align note counting and rejection handling (fix run)` on
  `notes/linear-GTB-2-20261018T171509Z`.
