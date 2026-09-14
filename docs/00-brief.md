# Δίοδος / Diodos

**An additive scaffolder with a refusal engine and a verdict pack.**
> **Partially superseded.** Written before zero retention (`docs/09`), the browser runtime
> (`docs/12`), the L1-Greek correction (`docs/13`) and the pack architecture (`docs/14`).
> Its reframe, its pedagogical positions and its honest case against building are current.
> Its storage model, its stack, its ledger, its market framing and its "six things only he
> can decide" are not — see `docs/15-backlog.md`.

---

## 0. The reframe, stated once

The original idea — teacher inputs a diagnosis plus their content, app returns differentiation — is wrong on both halves.

**The input is wrong.** A diagnosis label is the worst available conditioning variable. Within-label heterogeneity exceeds between-label variance; comorbidity makes label-keyed lookup self-contradictory (ADHD says vary and chunk, ASD says keep it identical — only a barrier vector can arbitrate); and most struggling students in Greece have no γνωμάτευση and never will. There is no diagnosis field in this system. Not write-only, not converted-at-ingest: it does not exist as a column, a form field, or a file format.

**The output is wrong.** A tool that rewrites the teacher's text competes on the one axis frontier models improve fastest, and it forces O(document) re-verification from a professional who must sign the result. The app's two jobs are to **add** to his material without touching a character of it, and to **refuse** — with a named, task-specific reason — the moves that would invalidate the assessment.

> *"I did not add a pre-filled OPVL grid, because Diii requires the student to name the limitation."*

That sentence is the product. A chatbot cannot say it, its failure mode as models improve is *more fluent* construct contamination, and it is what an MYP coordinator authorises a tool for. The rewrite is the bait you give away.

---

## 1. What is durable and what is not

| Durable | Why | Not durable |
|---|---|---|
| Greek demand analyser (syllabifier, genitive chains, nominalisation, λόγιες μετοχές, «Να +» command terms) | Measurement is not generation; a better model does not produce proof | Prose fluency |
| Refusals tied to what *this* task assesses | Better generation → more fluent contamination → refusal appreciates | Prompt engineering |
| The accumulating record of refusals, overrides, reapplications | Institution-specific, human-attested, non-derivable | Formatting fidelity (12-month lead) |
| Thirteen deterministic validators | Proof, not plausibility | Tool count, strategy libraries |

**Corollary that governs the build order:** the durable parts start in week 2, not month 6. The wedge ships the vehicle; the cargo loads from ordinary use.

---

## 2. Data model

*(See `data_model_sketch` for the full DDL — reproduced here in summary with the design rules that matter.)*

**Storage tiers:** text files he owns (`classes/`, `glossaries/`, `strategies/`, `ontology/`, `verdicts/`, `preferences.md`, all in a private git repo) · immutable blob store for his uploads · SQLite for runs, ops, verdicts and the hash chain. `tenant_id` on every table from migration `0001` plus query discipline through one `repo(tenant_id, …)` function, so the month-6 Postgres + `FORCE ROW LEVEL SECURITY` port is mechanical. That costs two hours now and is unrecoverable later.

**Three rules enforced by the class-file parser:**
1. `+ strengths` is mandatory, minimum one, or the learner line fails to parse. A profile that is a list of deficits produces deficit output.
2. Language-bound codes carry an `el:` / `en:` / `grc:` prefix. `el:decode.rate=2` and `en:decode.rate=0` are different facts about the same child — and in Greek the dyslexia signature is **rate plus inflectional-ending spelling**, never accuracy, because Greek is shallow for reading and deep for spelling.
3. `[evidence date]` is **optional**, and this is a deliberate reversal of the obvious design. An unevidenced barrier still generates moves, watermarked `provisional`. What evidence gates is **permission**, not generation: a provisional move cannot write a ledger event, cannot enter an export pack, cannot touch `summative_internal`, and cannot authorise an arrangement. The UI offers one-click *"record this observation"* at the moment of acceptance — when the teacher has just seen evidence of the barrier in their own document, the highest-yield moment there will ever be. A generation-time evidence gate is ritual: trivially gamed by typing a date, and it withholds output on exactly the undiagnosed majority the product claims to serve.

**The ontology is a join key, not IP.** Say that out loud. Three independent designs produced 20, 24 and 31 codes with incompatible boundaries, which is itself evidence the boundaries are arbitrary. Run the **discrimination test on day 3**: for every code pair, does any strategy card select on one and not the other? Merge every pair that fails. Expect 12–15 survivors. Anchor each to an ICF-CY code and one teacher-answerable observable question ("loses the thread after about three sentences") — a teacher cannot answer *"phonological deficit?"* and can answer that.

**Affect codes (`evaluative.threat`, `predictability.need`) cannot reach a text transform.** Enforced at card-load time: any card whose targets include an affect code and whose `execution_kind` is not `teacher_action` or `render_directive` fails to load. Emitting a chunking transform for an anxiety barrier is the most common category error in existing tools and it is structurally impossible here. Anxiety is answered by known assessment conditions, low-stakes retrieval, audience choice and graded exposure.

### The verdict table — re-founded

This is the moat, and the naive version of it is wrong. MYP strands are deliberately generic framework-level phrases; what is actually assessed is instantiated by the **task-specific clarifications** the teacher writes, plus the statement of inquiry. A global verdict "word bank invalidating on I&S Ai" is either trivially coarse or wrong for half of real tasks, every coordinator will overrule 10–20% of it, and 400 rows keyed to paraphrased strand descriptors is a derivative work over IB copyright material.

So:

- **Key on `(strategy × standards_pack × subject_group × strand_id × product_type × assessed_verb)`.** Store strand *identifiers* plus **our own rationale**. Never paraphrase IB descriptor text into the database.
- **Seed ~15 rows**, covering this week's actual tasks. Not 400.
- **Asymmetric defaults:** missing row → `conditional` (flag and proceed) on teaching/formative; `invalidating` (fail closed) on `summative_internal`. Fail-closed everywhere makes the tool inert outside the corridor he hand-built, which is the corpus-first failure in disguise.
- **Every refusal renders an override control that writes a `verdict_candidate` row** with the teacher's typed reason, the task, the strand and the timestamp. The refusal log *is* the authoring queue. That is how 15 rows becomes 400 without a curation sprint and the only mechanism that scales verdict authorship past one person.
- **Track `refusal_rate_per_artefact` as a product alarm.** Above ~20% on non-summative, the seed is mis-specified, not the teacher wrong.
- **The cross-tenant product is a versioned verdict pack** (a subscription, like a curriculum publisher's). The defensible asset is *the school's recorded access-policy decisions in a form IB evaluation accepts* — which no competitor copies and no coordinator overrules, because it is theirs.
- **A fourth verdict value exists: `criteria_modification`.** IB does permit modified MYP criteria for documented needs in defined circumstances, with eligibility consequences. Refusing it entirely is factually wrong and looks naive; allowing it silently is worse. Gate it to coordinator level, require a typed rationale, print the eligibility consequence on the artefact, and write a distinctly-typed ledger row.

---

## 3. Pipeline

*(Full step table in `pipeline`. The four decisions that carry it:)*

**Determinism boundary.** Syllabification, sentence segmentation, command-term extraction, demand measurement, strategy eligibility, construct legality, strand coverage and every validator are **deterministic code**. The LLM does exactly two things: emit span-anchored edit operations under a strict schema, and write teacher-facing rationale. It never selects a strategy and cannot name one, because the schema has no field for it — name-dropping is structurally impossible rather than penalised.

**Additive by default.** Gloss-in-place, colometry, organisers, discourse stems, command-term unpack, blank OPVL grid, worked-example triple, layout directives. All verifiable at a glance because the original text is untouched. **Rewriting sits behind an explicit per-block "rewrite this paragraph" action, hard-capped at ≤12 accepted rewrite ops per artefact, rendered as side-by-side paragraph pairs** — because the unit of judgement for a rewritten Greek paragraph is the paragraph, and O(changes) verification collapses to O(document) on exactly the dense Greek source that justified building a Greek analyser.

**Three text-transform classes with three different legality profiles**, never collapsed into one "simplify" operation: *syntactic de-densification* (split clauses, resolve pronouns to referents, front main clauses, de-passivise — preserves every proposition, always safe); *lexical glossing* (keep the tier-3 term, add apposition — safe); *lexical substitution* (replaces the term — **frequently invalidating**, because in I&S the term is the assessed knowledge under Ai). Most "simplify this text" tools collapse all three. Don't.

**Code proposes routes; the teacher assigns.** Deterministic gap-cut into ≤3 access routes named by access route («Με οδηγό» / «Βασική» / «Ανοιχτή»), min cluster size 3, plus an extension lane and a named individual lane with the coverage gap stated out loud. But the system **never writes an automatic student→route mapping**: chips are draggable and the record says "teacher assigned X to route B", named actor, timestamp. The unit of generated output is the **class artefact set**, not the student. One line of design, and it is the difference between content adaptation with human assignment and automated steering under Annex III 3(b).

**Per-item strand tagging only on `summative_internal`.** On teaching and formative, three clicks tag the document with criteria and volume-reduction moves are disabled with a stated reason. Auto-propose item tags from command term, marks and item text; require confirmation on ≤4 items per sheet. A 12-item worksheet is 3–5 minutes of genuine cognitive work at 23:10, against a manual baseline of 25–35 minutes — tagging plus mode declaration plus review can eat the entire claimed saving on exactly the documents where the tool should be worth most.

**Two renders from one source, always.** Student copy: zero diagnostic marking, zero barrier codes, zero route name a student could read as a rank. Teacher copy: the full rationale, evidence trail, fade dates and refused list. Dignity is a product requirement.

---

## 4. What the model is asked for, exactly

```
SYSTEM (frozen, cached, English even for Greek content):
You apply exactly one transformation to exactly one span of teaching material.
You do not choose strategies. You do not add pedagogical commentary.
You do not add information absent from the provided spans.
Rules for {strategy_id} v{n}: …
Preserve verbatim, character for character: {protected_tokens}.
Preserve the command term and the object it governs.
Do not change the language. Output language: {output_language}.
Output: SpanEdit[] conforming to the schema. Nothing else.
```

Absent: the diagnosis, the student, a strategy menu, "be helpful", "consider the student's needs". The barrier vector has already done its work in selection and re-enters only as numeric parameters (`target_wps`, `chunk_size`, `max_new_vocab`).

Routing: `claude-opus-5` for all Greek and all generative work (Greek's ~2.5× token penalty lands on **input**, which caches at 0.1×, so the marginal cost of Opus over Sonnet on Greek is small while the quality delta on accentuation, register and terminology is largest); `claude-sonnet-5` for English mechanical; `claude-haiku-4-5` never on the Greek path. Four cache breakpoints, `sort_keys=True`, no timestamp anywhere in a cached prefix, NFC normalised at ingest (mixed normalisation is a silent cache invalidator), and an integration test asserting `usage.cache_read_input_tokens > 0` — that failure is silent, permanent, and 3× on the bill.

---

## 5. Greek, specifically

Flesch–Kincaid and Lexile are invalid for Greek — inflectional morphology inflates syllable and word length without inflating difficulty, so the numbers vary with morphology rather than with demand. **v1 ships no Greek band number.** It reports the three driving features with the offending span quoted: *«τρεις αλυσίδες γενικής, βάθος 4 · ονοματοποίηση 7,2/100 · δύο λόγιες μετοχές (-θείς)»*. More actionable than "B1", and honest about what it knows.

If a calibrated ordinal model ever ships (month 4+, on the ΙΤΥΕ Διόφαντος grade-labelled corpus), validate it on **≥300 pairwise comparisons from ≥3 φιλόλογοι**, report inter-rater agreement *before* model-vs-consensus agreement, and never gate on Spearman ρ against one person's ranking of 30 passages — ρ from n=30 carries a ±0.2–0.25 interval, and validating an objectivity claim against your own ranking measures agreement with you. Check the corpus licensing before training on it.

**Ancient Greek and Latin: never generated.** Colometry (one κῶλον per line — the highest-leverage ΑΕ transform and philologically principled rather than a concession, executed as *index-based splits so the characters are never retyped*), interlinear morphological gloss giving **form not meaning** from a real analyser (Morpheus/CLTK), a published aligned translation, known-lexicon highlighting. The LLM writes only the Modern Greek scaffolding around it, and `AG-01` drops any ΑΕ string that is not an exact NFC substring of the source. One wrong perispomeni in front of a φιλόλογος ends the product's credibility permanently, and `EL-01` (hunspell `el_GR` + minimal-pair guard on πότε/ποτέ, πώς/πως, ή/η, τι/τί) is blocking for the same reason.

Greek command terms are **not imperatives** — they are periphrastic «Να + subjunctive» (`Να αξιολογήσετε`, `Να τεκμηριώσετε`) plus bare nominals (`Αιτιολόγηση`). Any extractor that matches imperatives finds nothing.

---

## 6. The single metric, and the kill rule

**Unedited Reapplications per Week (URW).** Count, per teaching week, of accepted ops where the same strategy card was applied to the same class or student-set on a *later* document and accepted **without edit**.

- **Target: URW ≥ 4 sustained for six consecutive weeks by week 16.**
- **Kill rule, written in the README on day one: URW < 2 for three consecutive weeks after week 8 → stop building.**

Why this and not the obvious alternatives. Moat's "closed access loops per student per term" and Institutional's PCR-3/14 are both *renewal* metrics: they cannot move for two terms and therefore steer nothing in weeks 1–12. Both also depend on humans volunteering post-hoc outcome data, which has a single-digit capture rate in every SEN progress-monitoring product of the last twenty years — and where it happens, it is a teacher's judgement about an adaptation they themselves chose, so `P(worked)` converges to ~0.85 everywhere and any prior fitted on ~300 events per term is fitting noise while calling itself a flywheel.

URW is unfakeable, free to collect, and it is simultaneously zero unless the output was good enough to accept, used in a real room, and *still trusted the second time*. Human outcome capture is demoted to exactly one prompted item per week from the fade-review queue: one tap, three options. No learned prior in v1; outcome data breaks scorer ties only above n≈500 events.

Secondary instruments, tracked and never optimised: accept-rate band 65–85% (above 95% is rubber-stamping, below 50% is bad output), median upload→export under 6 minutes against the stopwatched manual baseline, `demand_restore` rate (teacher putting difficulty back — rising means over-simplification), `refusal_rate_per_artefact`, and a per-student ceiling audit that fires when nearly all of a teacher's ops reduce demand. That last one is the equity ratchet and it is the only standing empirical check that differentiation is not quietly becoming permanent lowered expectation.

**And one test that is not a metric.** Week 8: hand the laptop-local build to **two teachers at a different school** who owe him nothing. No demo, no onboarding, no support. Does either export a printed pack in week 2, unprompted? His own usage and his IMS colleagues' usage are structurally false positives — he built it, they are his colleagues. Write this test and its kill condition into the README *before* any application code.

---

## 7. Pedagogical positions, taken

- **Refuse to implement:** learning styles / VAK (Pashler et al. 2008 — no crossover interaction has ever been found), Irlen overlays (Griffiths et al. 2016; 2016 UK consensus), dyslexia-specific fonts (Kuster et al. 2018 — OpenDyslexic gives no advantage). Ship them as library rows graded `insufficient` / `contraindicated` with the citation, on a **Refusals page**, alongside what does work: increased letter and word spacing (Zorzi et al., PNAS 2012) and line-length control. Teachers will ask for all three; the honest answer is a feature.
- **Never answer a writing barrier with more grammar practice.** Graham & Perin 2007: grammar instruction d≈−0.32. Hard-code it as `contraindicated`.
- **Evidence grade is per `(card × barrier)`, not per card.** Spacing is `strong` for `decode.rate` and `insufficient` for `wm.verbal`. One grade per card is how "evidence-based strategy" laundering happens.
- **Every scaffold is typed at generation:** `atl_taught` (with a fade schedule and review date), `permanent_arrangement` (with issuing authority and expiry), or `universal_design`. Untyped, un-faded scaffolds are not shippable output. This is the reframe an MYP coordinator signs off on: *a scaffold with a fade plan is ATL instruction; a scaffold without one is an access arrangement.* It converts undocumented kindness into a documented ATL development plan and a documented access record.
- **Class-wide uplift before individual overlay, always**, with the payoff number on screen every single time: *«Μετά την καθολική προσαρμογή, 3 μαθητές χρειάζονται ακόμη τη διαδρομή "Με οδηγό" (ήταν 7).»* Teachers make one better version plus two overlays; nobody makes 28 versions. Getting this split wrong kills adoption in week three.
- **Extension lane on every artefact by default**, and extension means conceptual transfer — same criterion at 7–8, a new global context, a harder command term — never more questions. `CE-01` (top level still reachable) is a blocking validator, because a differentiated task that caps a student at level 5 is a different task.
- **Arrangements are modelled per issuing authority and never merged.** A student in Greece can legitimately hold conflicting ΚΕΔΑΣΥ and IB sets with different scope and expiry. Merging them is the most likely way this product causes real harm: extra time given because the app suggested it, without IB authorisation, is an invalid assessment and a maladministration finding. And surface the mismatch Greek practice misses — προφορική εξέταση is granted almost reflexively, is correct for transcription and Greek spelling impairment, and is **actively harmful** for word-retrieval and social-evaluative-anxiety profiles, where it converts a writing barrier into a speaking barrier.

---

## 8. Build sequence

*(Full detail in `build_sequence`. The three non-negotiables:)*

1. **Day 1 is the premise-falsification test.** XML-canonical equality of untouched `<w:p>` elements after normalising rsid and proofing attributes, on **20** of his real documents — not 3, not byte equality (python-docx rewrites the package). Same day: ship the named-refusal list for text boxes (`w:txbxContent`, extremely common for answer spaces in Word-authored Greek worksheets), OMML equations, SDT content controls and SmartArt. Write edits by rebuilding the run sequence from a captured formatting template — Word splits one sentence across 5–8 runs on rsid, proofing state and `w:lang` boundaries, so in-run mutation silently destroys formatting.
2. **Day 2 evening is the naive baseline**, before any curation cost is sunk: command-term downgrade rate, tier-3 retention, profile-swap Jaccard on 12 golden items. Expect ~30–50% downgrades, ~0.7 retention, ~0.8 Jaccard. Those three numbers on one slide are the entire business case and it will never be this cheap to obtain again. Same evening, stopwatch ten real tasks by hand.
3. **End of week 2 is first real classroom use, printed and handed out.** Not month 3. If it slips past week 4, something in the scope is wrong, not the schedule.

Read every day count as a working day for a man with a full teaching timetable — "week 1" is realistically three weeks of evenings. The order is what matters; every day is independently falsifiable.

---

## 9. The honest case for not building this

Read this before week 1, not after month 6.

**Every durable element is either something a frontier model erodes within twelve months, or something better delivered as a document than as software.** The docx round-trip — the whole wedge — is an I/O ergonomics problem, and agentic models with file-editing tools are closing it faster than a part-time solo build ships.

**The moat may be a PDF, not a product.** If the verdict judgements are good, he can publish them in three weekends as a versioned bilingual reference with a ten-page guide. It gets read, cited and argued with by MYP coordinators in thousands of schools, establishes exactly the authority the software was meant to monetise, and costs no infrastructure, no DPA, no RLS migration, no incorporation and no PI insurance — and no model release obsoletes it. **If the judgements are not good enough to publish under his own name, they are not good enough to sell to a school either.** That is the sharpest available test of whether the asset is real.

**The ledger business depends on humans entering outcomes, and they will not.** Twenty-year base rate. This design derives the signal from reapplication instead, which is honest — but it means the "system of record with switching costs" story is thinner than it sounds, because a record that accrues evidence without improving the product is lock-in, not compounding.

**The market is small and the buyer has no budget line.** Thirty to fifty plausible Greek private/international IB customers at €1,200–8,000; total domination is a sub-€500k/yr business reached in year four by one person with a full timetable. Greek public schools have no procurement route at all.

**The validation loop is closed.** He will use it because he wrote it; his colleagues will try it because he asked. Nothing in this plan generates an uncontaminated signal except the week-8 external test, and the honest prior is that it fails.

**And there is a real opportunity cost.** He already ships a teacher-substitution app with school-ops customers and a distribution path into Greek schools — roster, auth, tenancy, DPA and the relationship all exist. An access/differentiation module inside that product reaches more students in six months than a greenfield platform reaches in two years, and skips every objection above.

**The version that survives all of this is the one specified here:** local, single-teacher, no server, no student database, no ledger, no institution, three weeks of evenings, whose sole purpose is to discover whether span-level review of his own Greek documents is something he still does in week six. If that habit forms, everything else is worth arguing about. If it does not, he has spent three weeks and published a reference document — not a year and an ΙΚΕ.

---

## 10. Six things only he can decide

1. **Which market**, in month 1, before the port. Global IB world schools (English-first, MYP/DP packs, no ΚΕΔΑΣΥ modelling, Greece as pilot not market) — or individual teachers at €12–18/month with no institution in the loop — or Greek-institutional. **Do not choose the third.**
2. **What he actually repeats weekly** — subject, language, artefact type, and print versus digital. Build the week-1 slice for exactly that one workflow; if IMS is Classroom-digital, the print pack and the A6 card lose their viral role and the export surface changes in week 2.
3. **SEN versus Greek-L2 versus English-L2** in his actual rooms. If the L2 cohort is larger — and in international schools it usually is — bilingual glossing and register scaffolding is a bigger v1 lever than any SEN scaffold, and the product is "academic language access", a different sale to a different buyer.
4. **Is there a coordinator who will author and sign verdict rows**, and does the school's assessment policy already distinguish authorised arrangements? If no on both, the bus factor is terminal and the reference document (§9) is the better artefact.
5. **Who holds the γνωμάτευση at IMS**, and can a subject teacher see it at all? If not, barrier-first is the only viable input — fine — but the institutional ledger has no authoritative source and must not be sold as one.
6. **The manual baseline.** Ten real tasks, two real profiles, a stopwatch, week 1. Nobody else can produce this number, and without it there is no value claim and no way to read any later metric.
