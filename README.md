# Δίοδος / Diodos

An additive scaffolder with a refusal engine and a verdict ledger.

Teachers hand it their own material. It **adds** access scaffolds without touching a
character of the original, and it **refuses** — with a named, task-specific reason —
the moves that would invalidate the assessment.

Scope: **History, Literature, Modern Greek** and the Greek-as-L2 cohort, across **MYP and
Γυμνάσιο**, bilingual EL/EN. **Nothing about a student is ever stored.**

> *"I did not add a pre-filled OPVL grid, because Diii requires the student to name the limitation."*

That sentence is the product.

## Status

Design phase. Zero application code. Nothing below is committed to until the
premise-falsification tests in [`docs/04-build-sequence.md`](docs/04-build-sequence.md)
pass.

## The two reframes

1. **No diagnosis field.** Not write-only, not converted-at-ingest — it does not exist as a
   column, a form field, or a file format. Within-label heterogeneity exceeds between-label
   variance, comorbidity makes label-keyed lookup self-contradictory, and most struggling
   students have no diagnosis and never will. Key on an evidenced, per-language **barrier
   vector** colliding with a **measured demand profile** of the specific paragraph.
2. **Additive, not rewriting.** Rewriting competes on the axis frontier models improve
   fastest and forces O(document) re-verification from a professional who must sign the
   result. Rewrite sits behind an explicit per-block action, capped at ≤12 ops per artefact.

## Zero retention

The diagnostic document never enters the app — no upload, no paste, no OCR, no code path.
The teacher reads the γνωμάτευση they are already entitled to read and **ticks boxes**.
Barrier profiles live in session memory and die with the tab. Only the teacher's own
material persists, in files they own.

> *"The app never sees the diagnosis. The teacher reads it and ticks what it means for this
> lesson. Nothing about a student is stored anywhere, ever."*

This deletes the DPIA, the DPA, the ΕΔΥ approval and the parent objection — and it kills
the access-ledger and institutional-sale directions permanently. Accept the trade.
See [`docs/09-no-storage.md`](docs/09-no-storage.md).

## Kill rule

**Unedited Reapplications per Week (URW) < 2 for three consecutive weeks after week 8 → stop building.**

Plus one external falsification test in week 8: hand the build to two teachers at a
different school who owe nothing. If neither exports a printed pack in week 2 unprompted,
the premise is wrong.

## Documents

| | |
|---|---|
| [`docs/00-brief.md`](docs/00-brief.md) | The brief. Start here. |
| [`docs/01-data-model.md`](docs/01-data-model.md) | Entities, the barrier ontology, the verdict table |
| [`docs/02-pipeline.md`](docs/02-pipeline.md) | Ingest → analyse → match → generate → validate → render |
| [`docs/03-stack.md`](docs/03-stack.md) | Stack decisions |
| [`docs/04-build-sequence.md`](docs/04-build-sequence.md) | Week-by-week, premise tests first |
| [`docs/05-compliance.md`](docs/05-compliance.md) | GDPR Art. 9, EU AI Act, Greek statutory context |
| [`docs/06-red-team.md`](docs/06-red-team.md) | Every objection, and the honest case for not building this |
| [`docs/07-constraints.md`](docs/07-constraints.md) | Hard constraints from the domain analysis |
| [`docs/08-alternatives.md`](docs/08-alternatives.md) | The three directions considered and how they scored |
| [`docs/09-no-storage.md`](docs/09-no-storage.md) | The zero-retention architecture |
| [`docs/10-standards-packs.md`](docs/10-standards-packs.md) | MYP vs Γυμνάσιο — why the same move gets two verdicts |
| [`docs/11-subject-playbooks.md`](docs/11-subject-playbooks.md) | History, Literature, Modern Greek, Greek-as-L2 |

## Permanently excluded, at every version

Inference or suggestion of a diagnosis, SEN category or support tier · grading, marking,
criterion levelling, attainment prediction · placement, streaming or παράλληλη στήριξη
recommendation · at-risk flagging · any inference of emotion, engagement or effort ·
transformation of external summative content · generation of Ancient Greek or Latin ·
any student-facing surface · any automated statement characterising a child · learning
styles/VAK, Irlen overlays and dyslexia fonts as evidence-backed options · any secondary
use of school data.
