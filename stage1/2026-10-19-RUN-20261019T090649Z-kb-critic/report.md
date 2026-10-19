# kb-critic report — RUN-20261019T090649Z-kb-critic

## What I was asked to do

Check Stage 1 PR #1 on branch `notes/linear-GTB-2-20261018T171509Z`: the new
Linear input `inputs/LIN-GTB-2.md` and every change the synthesizer made from
it. Question mode `decide` (D-160): judge the synthesizer's recorded defaults
rather than reopen them; client questions never block.

## What I did

- Read `inputs/LIN-GTB-2.md` (new on this branch; main has no earlier copy,
  so no `git diff` reading was needed). It has no `designs` front matter and no
  attachments, so nothing to open with the `office` helper or as images.
- Read the full PR diff against `origin/main`: `decisions.md` (DEC-0001 to
  DEC-0007), `glossary.md`, `roles.md`, `prd/modules/{orders,web,mobile}.md`,
  `prd/requirements/FR-0001..FR-0003.md`, `questions/Q-0001.md`,
  `questions/Q-0002.md`.
- Compared every quotation in the PRD with the note, sentence by sentence.
- Checked the Definition of Ready parts CI can't: observable Then clauses,
  example-table arithmetic (checked with `node`: 44 and 20 are right), roles.
- Could not read Globuli's generic checklist (`agents/kb-critic/checklist.md`)
  or `docs/requirement-lifecycle.md`: both live outside the paths this run may
  read, and a subagent's attempt was denied too. The project's `checklists/`
  folder holds only its README. I applied the checks named in the role
  constitution and the common-sense checks below instead.
- Filed no new question: the two client-side points (how the warehouse sees
  the note, how the printer counts) are already Q-0001 and Q-0002, and every
  other gap is one a synthesizer default can settle. Made no commits.

## Reading of the note (`inputs/LIN-GTB-2.md`)

Nothing the client said is missing from the PRD, and nothing in the PRD
contradicts the note:

| Note (line) | Where it landed |
|---|---|
| 10–12 free-text note, customer fills in when placing, warehouse sees | FR-0001 story/context; DEC-0007 + Q-0001 for "warehouse sees" |
| 16–17 optional, web order form and app order screen, max 280 | FR-0001.AC-2/AC-3, FR-0002.AC-2/AC-3, FR-0003.AC-2/AC-3 |
| 19 stored with the order, returned when fetched | FR-0001.AC-1, AC-5, AC-6 |
| 20–21 web icon; "Mobile can show it" in My orders | FR-0002.AC-4 (DEC-0005); FR-0003.AC-4 (DEC-0006, "can" made a must) |
| 22–23 can't be changed; email support | FR-0001.AC-4, FR-0002.AC-5, FR-0003.AC-5 (DEC-0004); out of scope |
| 25–26 confirmation email out of scope | Out of scope in all three |
| 28 backend first | `depends_on: [FR-0001]` in FR-0002/FR-0003; orders.md narrative |

All eight quotations (FR-0001 ×5, FR-0002 ×2, FR-0003 ×2, orders.md ×1) match
the note word for word. The issue is in English; no translation to check.

## Decisions (decide mode)

DEC-0001 to DEC-0007 are recorded with date, decider and source; none
contradicts the note, and each is a sensible default. They stand. Two of them
are vague where they meet (DEC-0001 vs DEC-0003, F-01 below).

## Questions filed by the synthesizer

Q-0001 (warehouse) and Q-0002 (counting, line breaks) are well formed:
`blocks: []`, audience client, understandable without the PRD, and the
Croatian reads naturally ("prijelaz u novi red", "pisač naljepnica"). No
person is named. No duplicate between them.

## Findings

### F-01 — Medium — Field limit and server limit count differently; no criterion for a rejected note (FR-0002, FR-0003, DEC-0003)

DEC-0001 counts Unicode code points after trimming. DEC-0003 says the form
field "accepts at most 280 characters" without saying what a character is.
Flutter's length limiter counts grapheme clusters and HTML `maxlength` counts
UTF-16 units, so the two ends can disagree. A note of 280 family emoji
(5 code points each; `node` confirms) passes the app's field and orders-service
answers HTTP 400 (FR-0001.AC-3). Neither FR-0002 nor FR-0003 says what the
client does then. A default can settle it: define the field's count as
DEC-0001's, and add a criterion that a note orders-service rejects is shown as
an error and places no order. Also say whether a CRLF line break counts as one
character (DEC-0001 says "a line break counts as one").

### F-02 — Medium — FR-0001.AC-1 "alone or in the order list" lets the list omit the note

`prd/requirements/FR-0001.md:66`: "fetching that order, alone or in the order
list, returns the same note text". The "or" is satisfied by a single-order
endpoint alone, but FR-0002.AC-4 (icon in the web list) and FR-0003.AC-4 (text
in My orders) need the note, or at least its presence, in the list response.
Should read "both alone and in the order list".

### F-03 — Low — Whether the note field is multi-line is unspecified

DEC-0002 keeps line breaks and Q-0002 asks about them, but FR-0002.AC-3 and
FR-0003.AC-3 don't say whether the field lets the customer enter one. `web.md`
and `mobile.md` list no open questions although Q-0002 concerns their fields.

### F-04 — Low — Q-0001 and Q-0002 not linked from FR-0001's `open_questions`

`prd/requirements/FR-0001.md:12` leaves `open_questions: []` with a comment.
The role constitution expects client questions linked there with `blocks: []`
in decide mode. Medium confidence: I could not read the lifecycle doc, so if
CI's Definition of Ready rejects a non-empty list, the synthesizer's choice is
right and this finding falls away.

## Observations, no finding

- DEC-0005's "the list does not show the note's text" makes a web customer
  unable to read their own note after ordering (the app shows it). The note
  only asks for an icon, so the default stands; the client may want to revisit.
- FR-0001.AC-3's Then also states that exactly 280 is accepted, which AC-1
  already covers; harmless.

## Verdict

`fail`: two Medium findings for the synthesizer. No question filed, no
`clarifying` set, no commits.
