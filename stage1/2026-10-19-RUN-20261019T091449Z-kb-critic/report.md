# kb-critic re-check — RUN-20261019T091449Z-kb-critic

Stage 1 PR #1, branch `notes/linear-GTB-2-20261018T171509Z`, iteration 2,
question mode `decide`. Input: `inputs/LIN-GTB-2.md`.

## What I was asked to do

Re-check the two findings of RUN-20261019T090649Z-kb-critic after the
synthesizer's fix run (commit fbf2c2e), and check only the changed text for
new problems.

## What I did

- Read the fix commit's diff (`decisions.md`, FR-0001, FR-0002, FR-0003) and
  the current state of those files, the issue file and Q-0002.
- Checked the new decisions DEC-0008 and DEC-0009 against the issue: neither
  contradicts it. The issue only says "Max 280 characters (the warehouse
  label printer cuts off after that)"; the counting unit, CR LF handling and
  the clients' behaviour on a rejection are defaults the issue leaves open,
  and both are recorded with source and the "client may still change this"
  note.
- Verified the example rows' numbers with node: a family emoji is 5 code
  points, 56 of them are 280 and 57 are 285.
- Looked for project checklists: `checklists/` holds only its README, so only
  the role's generic checks apply.
- Did not extend Q-0002: it already describes the limit as characters (so "č"
  counts as one) after trimming, with line breaks counted as one; DEC-0008
  only sharpens that for the engineers and changes nothing the client needs
  to confirm.

## Earlier findings

### RUN-20261019T090649Z-kb-critic.F-01 — fixed

DEC-0008 makes the web form and the app count the same unit as orders-service
(code points after trimming) and normalises CR LF to LF at both ends. DEC-0009
says what the clients do on an HTTP 400 for the note. FR-0002.AC-3 and
FR-0003.AC-3 name the unit; FR-0002.AC-6 and FR-0003.AC-6 add the rejection
case with observable Thens (no order, error at the field in the user's
language, input kept). FR-0001.AC-3 and its examples name DEC-0008.

Note for the planner, not a finding: with both ends aligned, AC-6 can only be
exercised with a stubbed orders-service response; that is a legitimate test.

### RUN-20261019T090649Z-kb-critic.F-02 — fixed

FR-0001.AC-1 now reads "both when the order is fetched alone and when it
appears in the customer's order list; each order in the list carries its own
`deliveryNote`", with an example row for the list.

## New findings on the changed text

### F-01 (Medium) — The field's cut at 280 code points is undefined inside a grapheme cluster and around leading whitespace

`prd/requirements/FR-0003.md` lines 64–69 (same text in
`prd/requirements/FR-0002.md` lines 61–65). AC-3 now counts code points, "not
as grapheme clusters", and keeps the Then "the field holds only the first 280
characters". When the 280th code point falls inside a cluster, the two
natural implementations differ: keep the partial cluster (278 × "a" plus a
dangling man-plus-joiner, as `[...s].slice(0,280)` gives, shown as a broken
emoji or a letter without its accent) or drop the whole cluster (the field
holds 278). Likewise "after trimming" leaves open whether pasting three spaces
plus 285 × "a" leaves "   " plus 277 × "a" (first 280 code points as pasted)
or "   " plus 280 × "a" (280 after trimming). Neither DEC-0003 nor DEC-0008
says. A default can settle this; it is for the synthesizer, not the client.

### F-02 (Low) — FR-0001 example says the field shows 57 family emoji, which DEC-0008 rules out

`prd/requirements/FR-0001.md` line 124: "HTTP 400, no order created, although
the field shows 57 characters". Under DEC-0008 and FR-0002.AC-3/FR-0003.AC-3
the field holds at most 280 code points, so at most 56 family emoji; the
remark describes the grapheme-cluster behaviour the fix removed. The row is
right for a direct API call; only the remark is stale.

### F-03 (Low) — FR-0001.AC-1 "the same note text" omits the CR LF normalisation

`prd/requirements/FR-0001.md` lines 67–70: the Then promises "the same note
text (trimmed as DEC-0002 says)", but DEC-0008 and the example on line 125
store a CR LF as one LF, so a note sent with CR LF comes back changed. The
Then should say "trimmed as DEC-0002 says and with CR LF normalised as
DEC-0008 says".

## Decisions I made

- No question filed and no status changed: decide mode, and every new point is
  one a default can settle.
- No commit made: my only permitted edits are questions, links and
  `clarifying`, and none was needed.

## What I couldn't do

Nothing outstanding. Globuli's generic checklist file is not in this repo; I
applied the checks the role constitution lists.

## Verdict

`fail`: one Medium finding (F-01) and two Low ones. Both earlier findings are
fixed.
