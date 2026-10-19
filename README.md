# Shopfront runs

The written trail of every Globuli agent run on Shopfront (Globuli's brownfield testbed).
Written only by the orchestrator, committed straight to `main`. Layout and formats:
Globuli's `docs/git-conventions.md` and `schemas/run-record.schema.json`.

| Path | What |
|---|---|
| `work-items/WI-NNNN/<nn>-<agent>-<run>/` | `run.json` + `report.md` per run on a work item |
| `stage1/<date>-<run>/` | Stage 1 runs (synthesizer, critic) |
| `planning/<date>-<run>/` | Planning sessions |
| `nightly/<date>/` | Nightly suite results and triage |
| `release-check/<date>-<run>/` | Release checks |
| `digests/<date>.md` | The supervisor's daily digest |

Full transcripts are not committed; they stay on the laptop for 30 days.
