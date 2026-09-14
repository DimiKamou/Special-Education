# Δίοδος / Diodos

An additive scaffolder with a refusal engine and a verdict pack.

Teachers hand it their own material. It **adds** access scaffolds without touching a
character of the original, and it **refuses** — with a named, task-specific reason —
the moves that would invalidate the assessment.

Scope: **History, Literature, Modern Greek**, across **MYP and Γυμνάσιο**, for a room of
**L1 Greek / L2 English** students. A browser app with no login. **Nothing about a student is
ever stored** — and the deterministic moves never leave the device at all.

The engine is pack-agnostic: language packs, standards packs and bridge packs are data.
Adding IGCSE or Spanish is a file, not a rewrite.

> *"I did not add a pre-filled OPVL grid, because Diii requires the student to name the limitation."*

That sentence is the product.

## Status

Design phase. Zero application code, zero data assets. Nothing below is committed to until
the week-1 premise-falsification tests in
[`docs/04-build-sequence.md`](docs/04-build-sequence.md) pass — each one has a stated
threshold, and three of the five can kill the project.

**Document precedence.** `docs/00`–`08` predate three scope changes and were never edited to
match. The rule: **09 > 00–08 on storage · 12 > 00–08 on stack and trust boundary ·
13 > 10/11/00 on cohort · 14 > 03/04 on layout.** `docs/03` and `docs/04` have been
rewritten; `docs/06`, `07` and `08` are marked archival.

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
| [`docs/12-site-architecture.md`](docs/12-site-architecture.md) | It is a site: trust boundary, stack, screens, adapt vs generate |
| [`docs/13-l1-el-l2-en.md`](docs/13-l1-el-l2-en.md) | The actual cohort: L1 Greek, L2 English — cognate bridge, command-term trap |
| [`docs/14-going-global.md`](docs/14-going-global.md) | Three pluggable packs, one engine |
| [`docs/15-backlog.md`](docs/15-backlog.md) | **Ranked backlog. Start here to build.** |

## Permanently excluded, at every version

Inference or suggestion of a diagnosis, SEN category or support tier · grading, marking,
criterion levelling, attainment prediction · placement, streaming or παράλληλη στήριξη
recommendation · at-risk flagging · any inference of emotion, engagement or effort ·
transformation of external summative content · generation of Ancient Greek or Latin ·
any student-facing surface · any automated statement characterising a child · learning
styles/VAK, Irlen overlays and dyslexia fonts as evidence-backed options · any secondary
use of school data.
