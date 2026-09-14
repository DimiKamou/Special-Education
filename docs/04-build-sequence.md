# Build sequence

> Rewritten. The previous version predated zero retention, the browser runtime, the
> L1-Greek correction and the pack architecture. It scheduled `0001.sql` with `tenant_id`
> on day 1, a `python-docx` round-trip, a class-file parser, a hash-chained trace, a
> month-3 Postgres + RLS port, an Art. 28 DPA, a DPIA pack and an Access Change Log —
> all of which `docs/09` and `docs/12` killed. Do not restore it.

**Read every "day" as one working evening for a man with a full teaching timetable.**
The order is what matters. Every item below is independently falsifiable.

---

## Week 1 — five evenings, three of which can kill the project

Nothing here is a feature. All of it is an attempt to destroy the premise cheaply.

- **E1 · The corpus, then the round-trip.** `fixtures/real/` with **20 of your own .docx**,
  stratified: ≥4 bilingual EN-with-Greek-margin, ≥3 you know are messy, ≥5 authored by
  *colleagues in Word* rather than exported from Google Docs, spread across History-MYP-EN /
  Λογοτεχνία-EL / Νεοελληνική-EL. Twenty exports of one template test one thing twenty
  times. `scripts/survey.ts` prints per document: `w:txbxContent`, `m:oMath`, `w:sdt`,
  shapes, `w:ins`/`w:del`, `altChunk`, header/footer parts, `w:numPr`, runs-per-paragraph
  distribution, `w:lang` switches per paragraph. Commit it as `docs/15-fixture-survey.md`.
  Then `engine/docx/pkg.ts` and the **no-op canonical round-trip**, with the canonicalisation
  spec written *first*. **Pass = ≥17/20 documents canonical-equal on every body `<w:p>`.**
  Verify in Word on Windows *and* Mac; never LibreOffice, which is forgiving enough to give
  a false pass.
- **E2 · `project()` / `unproject()`.** Dual `raw` + `nfc` strings with an index map, tested
  over every `<w:p>` in all 20 fixtures — roughly 1,500 real paragraphs. `raw` is the sole
  offset domain for any `SpanEdit`; `nfc` is the analyser and validator domain. Exclude
  `w:delText` and the `mc:Fallback` branch. **This is the riskiest assumption in the project
  and the previous plan did not contain it.** Not green → no feature work. A stop, not a
  warning.
- **E3 · `annotate` + `split` for real.** Apply to all 20, open every one in Word on both
  platforms. **Repair-dialog count must be zero.** First artefact you can show a colleague:
  your own Παπαδιαμάντης passage in κῶλα, one κῶλον per line, your fonts and header intact.
- **E4 · `insert_after` + the Art. 50 footer.** Clone-based node building, `w:numPr` strip,
  direct-formatted tables, `DOCX-ORDER-01` as a post-write assertion. Define `Node` and
  `RenderDirectives` while writing it — both are referenced in `docs/01` and defined nowhere.
  Settles a question no doc has answered: OOXML has no word-spacing property and no measure
  control short of page margins, so **the print pack, not the .docx, is the faithful render
  for render directives.**
- **E5 · Both baselines, decision rule written before the run.** Twelve golden items (6 EL,
  6 EN) through a naive single prompt: command-term downgrade rate by term identity, tier-3
  retention against a hand-typed 40-term glossary, content-ablation Jaccard. Then stopwatch
  yourself differentiating **10 real tasks by hand**, per task, Word fiddling included,
  against two fixed tick-sets written down in advance. Commit `docs/16-baseline.md`. The
  manual median is the denominator of a kill condition that exists nowhere yet: median
  drop→print above 40% of it means the saving is not there.

**Weekend · the highest-yield content asset.** `packs/` skeleton plus **one** file: 60 MYP
command terms (EN term, rank, EL gloss, EL «Να +» pair, one *"what the examiner wants"* line
in your own words, and the `trap` field) and the first 40 false friends. Settle the IB
copyright line in `docs/10` before writing a row. Pure typing, zero tokens, and it is
publishable standalone — which puts half of the reference-document hedge in hand by day 7.

---

## Week 2 — the additive loop, ending in a real room

- **E6–E7 · Screens 1 and 2.** Next.js skeleton, drop zone, MYP↔Γυμνάσιο toggle, subject,
  assessment mode. Then Read: your command terms highlighted in your own text, tier-3 marked,
  the three driving features named. Greek analyser v0 in pure TypeScript. Write the feature
  specs before the code — `-ων` is claimed by the genitive-plural detector and the
  λόγιο-participle detector simultaneously and they will fight on every «των ανθρώπων»;
  nominalisation density needs a denominator, a finished suffix list and a stop-list for
  lexicalised nouns (πόλη, γνώση, κατάσταση); υπερβατό distance is undefined entirely.
  This screen is where the teacher decides whether you understood their document.
- **E8 · Screen 3, the room.** Barrier chips grouped by channel, session memory only.
  Complete the ontology first — 19 codes each needing an ICF-CY ref, one teacher-answerable
  observable question in *both* languages (they are the chip labels), a `language_bound`
  flag and severity anchors. Then stopwatch yourself re-ticking a real room and **merge codes
  until it lands under 15 seconds.** That stopwatch is a better discrimination test than the
  paper pair-wise one, because it merges codes no *teacher* can tick rather than codes no
  *card* distinguishes. Ship the negative test alongside:
  `expect(JSON.stringify(localStorage)).not.toMatch(barrierCodePattern)`.
- **E9 · Screen 4 and the ten zero-token cards.** Write the card schema first — it is
  currently scattered across five documents and collected nowhere. Name the ten: the docs say
  "seven operations" and "ten zero-token cards" four lines apart and never enumerate the ten,
  which blocks the first thing that works. Hardest is the highest-value Literature move: a
  docx margin gloss is a text box, a comment, a footnote or a two-column table, and text boxes
  are on the ingest refusal list.
- **E10 · Screen 5, export.** Two renders, .docx download, print pack via `@media print` with
  `print-color-adjust: exact` (without it the marginal gloss and known-lexicon highlighting —
  the two highest-yield moves — print white on the artefact that *is* the deliverable), `@page`
  A4, `break-inside: avoid`, per-route print views with counts. Plus the **student-copy
  sanitiser**: strip `word/comments.xml`, remove `w:ins`/`w:del`, reset core and app
  properties, emit neutral filenames. `9B-istoria-guided.docx` on a shared drive is SEN
  marking by filename, after all the care taken over footers and route names. Plus the
  pre-loaded demo document, which is the entire distribution mechanism and appears in no
  previous plan.

**Weekend · FIRST REAL CLASSROOM USE.** Printed, handed out, taught. The test is one clause:
*you did not open the exported file in Word and fix anything before printing.* A hand-fix
means the round-trip claim is false in practice whatever the canonical test said. Then write
down the three things that were wrong with the paper in the room.

---

## Weeks 3–4

- **W3 · The refusal engine.** Fix the three verdict-key defects first: pick one canonical
  strand-identifier form (the docs currently write `Diii`, `D-ii`, `Ai` and `A–D` for the same
  thing — use `myp.is.D.iii` everywhere); `strand_id NOT NULL` is unsatisfiable for Γυμνάσιο,
  which has no strands; and `assessed_verb` is in the key but no screen on the adapt path
  collects it. Then author the 15 seed rows **from your next three real summative tasks** —
  fifteen rows with a 100% hit rate on your actual work beats fifteen representative rows with
  a 5% hit rate. Watch `refusal_rate_per_artefact`: above 20% on non-summative the seed is
  wrong, not you.
- **W3 · The first model call, and only then.** One route, EU region, strict `SpanEdit`
  schema, four cache breakpoints, the `cache_read_input_tokens > 0` assertion, and the
  egress guard blocking *before* the fetch. Exactly one card: `S-DENOMINALISE-EL`. Nothing
  hits the server before the guard exists.
- **W4 · URW instrumentation**, in week 4 rather than at the end of month 1 — it is the kill
  metric and the week-8 evaluation needs four weeks behind it. `(strategy_card × typed
  class_handle)` counters in IndexedDB, plus `refusal_rate_per_artefact` and `demand_restore`.
  Then a **second real classroom use**: different class, different document. A zero there is a
  week-4 decision point rather than a week-11 surprise.

---

## Week 8 — the only uncontaminated signal

Hand the build to **two teachers at a different school** who owe you nothing. No demo, no
onboarding, no support. Does either export *or digitally assign* a pack in week 2, unprompted?

Your own usage and your IMS colleagues' usage are structurally false positives. Write this
test and its kill condition into the README before any application code.

---

## Month 2–3

Second subject pack. The bridge pack in full. The generate path. Answer-leak gate. Library to
40 cards. Verdict table grown from overrides rather than a curation sprint. The Art. 6(4)
classification memo and a one-page data-flow diagram — an afternoon each, and the only two
compliance artefacts that survive zero retention.

**Explicitly not in this plan any more:** Postgres, row-level security, multi-tenancy,
accounts, OIDC, Presidio, the Art. 28 DPA, the DPIA pack, the Access Change Log, the ΙΚΕ, and
PDF or scan ingest.

---

## The reference document — week 4, regardless of build state

Publish the command terms, false friends and verdict rationales as a versioned bilingual
reference under your own name. It is the **only uncontaminated external signal available
before week 8**, it is unaffected by any build outcome, and week 1's weekend already builds
half of it. Three weekends.
