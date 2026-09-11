# The zero-retention architecture

Constraint: **the diagnosis is protected; nothing is saved.**

This is not a limitation to work around. It deletes the DPIA, the Art. 28 DPA, the ΕΔΥ
approval, the parent objection and the whole institutional sale in one move — and it makes
the thing shippable by one person. Accept it and design *for* it.

## Three tiers of data, three fates

| Tier | Example | Fate |
|---|---|---|
| **Diagnostic document** | the γνωμάτευση PDF, a psych report, an IEP | **Never enters the app.** No upload, no paste, no OCR, no file picker. There is no code path. |
| **Barrier profile** | `el:decode.rate=3 · wm.verbal=2 · + strong oral reasoning` | **Session-scoped memory only.** Dies when the tab closes. Never written to disk, never in a log, never in a model payload. |
| **Teacher's own material** | worksheets, glossaries, strategy preferences, verdict rows, class shorthand | **Persisted, because it is his.** Local files in a directory he owns. Contains no student data by construction. |

The teacher reads the γνωμάτευση — which they are already lawfully entitled to read — and
**ticks boxes**. The app sees checkboxes, not clinical text. That is the entire privacy
design, and it fits in one sentence on a slide:

> *"The app never sees the diagnosis. The teacher reads it and ticks what it means for
> this lesson. Nothing about a student is stored anywhere, ever."*

## What this rules out, permanently

- Any student record, roster, identifier, or pseudonymised handle.
- The access ledger, the longitudinal profile, the "evidence pack for IB evaluation", the
  institutional sale. **The moat direction is dead.** Do not mourn it — it depended on
  humans voluntarily entering outcomes, which has a single-digit capture rate over a
  twenty-year base rate.
- Automatic carry-over between lessons. The teacher re-ticks, or pastes a barrier line
  from *their own* file. Re-ticking a profile is ~15 seconds; if it is more, the ontology
  is too big.

## What survives, and how

**The metric still works.** Unedited Reapplications per Week is counted per
`(strategy_card × class_handle)`, where `class_handle` is a string the teacher types
(`"MYP4B"`, `"Γ2"`). No roster, no student, no linkage — just "this card was accepted
again for this group without editing." Stores nothing about a child.

**The verdict table still works.** It is a table about *tasks and criteria*, not about
students. `(strategy × standards_pack × subject × criterion × strand × product_type)`.
Zero personal data. It is also the only thing left that compounds.

**Barrier profiles persist if the teacher wants them to — in their file, not ours.**
A plain text line they can keep in their own notes, paste in, and delete. The app offers
copy-to-clipboard and never a save button. Where that line lives is the teacher's
professional judgement about their own records, which is exactly where the decision belongs.

## The honest legal line

Zero retention does not mean "not processing." Transient processing of inferable health
data is still processing. What it means is:

- The app is **software on the teacher's device, operating on data they already lawfully
  hold**. There is no controller/processor relationship to paper.
- No special-category data is transmitted, stored, logged or retained. Barrier codes stay
  in browser memory; model calls carry only anonymous numeric parameters
  (`target_wps`, `chunk_size`, `max_new_vocab`) and the teacher's own text.
- Zero-retention terms on the model API, EU region, no training on inputs. Verify in the
  provider's data-processing terms, do not assume.

Say this before anyone asks. "We never store it" is a stronger position than "we store it
carefully", and it is only available if you actually never store it.

## The one thing you give up

No cross-session intelligence about a student. The app cannot say "this worked for her
last term." It never will. In exchange, it can be used tomorrow, by anyone, without a
single conversation with a DPO.
