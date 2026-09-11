# Generation pipeline

```
0 INGEST → 1 NORMALISE → 2 ANALYSE → 3 DECLARE → 4 COLLIDE → 5 VERDICT → 6 PROPOSE ROUTES
                                                                                    │
  11 LEDGER ← 10 EXPORT ← 9 REVIEW ← 8 VALIDATE ← 7 GENERATE ←─────────────────────┘
```

**0 · INGEST — CODE.** python-docx walks body + tables; each paragraph/cell becomes a `Block` with a ULID `bid` and a `(body_idx, para_idx, run_idx)` anchor. Named-refusal list shipped week 1: text boxes (`w:txbxContent`), OMML equations, SDT content controls, SmartArt, shapes → "I can only edit the main body of this document; paste the rest or accept body-only edits." Paste path and topic-only path produce the same shape. *Premise test (day 1): XML-canonical equality of untouched `<w:p>` elements after normalising rsid and proofing attributes, on **20** of his real documents — not byte equality, python-docx rewrites the package.*

**1 · NORMALISE — CODE.** NFC at the boundary, asserted (mixed NFC/NFD is also a silent prompt-cache invalidator). Final-sigma folding for indexing only. Per-block language ID by codepoint ratio. Greeklish detection blocks the run and asks.

**2 · ANALYSE — CODE. Zero LLM, ever.** Greek: own rule-based syllabifier (~120 lines, validated ≥99% against LibreOffice `hyph_el_GR`), genitive-chain depth, nominalisation density (-ση/-μός/-τητα), λόγιες μετοχές (-θείς, -ων/-ούσα), υπερβατό distance, polysyllable ratio, connective/comma subordination proxy. English: `textstat` reported as a **band**, never a decimal; tier-2/tier-3 against per-subject glossary files he maintains. **No calibrated Greek readability number in v1** — report the three driving features with the offending span quoted, which is more actionable and honest. Command terms against closed lexicons: ~60 EN with ordinal demand rank, ~35 EL including the periphrastic `Να + subjunctive` (Να αξιολογήσετε, Να τεκμηριώσετε) and the bare nominal (Αιτιολόγηση). Output: `demand[code] = (load 0-3, evidence_bids)`. Cached by `content_hash`, so re-running for another class is instant.

**3 · DECLARE — TEACHER, 3 clicks.** Mode, subject group, criteria. Per-item `criterion.strand` tagging is required **only** when `mode='summative_internal'`; on teaching/formative, volume-reduction moves are simply disabled with a stated reason ("I can't cut items without knowing which strand each evidences — tag them, or I keep everything"). Auto-propose item tags from command term + marks + item text; require confirmation on ≤4 items per sheet. `mode='summative_external'` → **hard stop**: return the access-arrangement checklist, the IB Access and Inclusion Policy reference, the ΚΕΔΑΣΥ διευκολύνσεις reference, and the IBIS deadline. Nothing else. Advertise the refusal.

**4 · COLLIDE — CODE.** `candidates = {s : ∃code. profile.severity[code] ≥ s.min_sensitivity ∧ demand[code].load ≥ s.min_load}` minus the contraindication graph, minus affect-channel text transforms (structurally impossible at card-load). **Evidence gates permission, not generation:** an unevidenced barrier still emits moves, watermarked `provisional`, which cannot write a ledger event, cannot enter an export pack, cannot touch `summative_internal`, and cannot authorise an arrangement. The UI offers one-click "record this observation" *at the moment of acceptance* — the highest-yield moment there will ever be.

**5 · VERDICT — CODE, asymmetric defaults.** Look up `(strategy, standards_pack, subject_group, strand_id, product_type, assessed_verb)`. `invalidating` → removed with the reason rendered. `conditional` → surfaced with the policy question. Missing row → `conditional` on teaching/formative, `invalidating` on summative_internal. **Every refusal renders an override control that writes a `verdict_candidate` with the teacher's typed reason** — this is how 15 rows becomes 400 without a curation sprint, and it is the only mechanism that survives one person's bus factor. Track `refusal_rate_per_artefact` as a first-class alarm: >20% on non-summative means the seed is wrong, not the teacher.

**6 · PROPOSE ROUTES — CODE proposes, HUMAN assigns.** Universal uplift first (the zero-token render/deterministic cards), then the payoff number on screen: *«Μετά την καθολική προσαρμογή, 3 μαθητές χρειάζονται ακόμη τη διαδρομή "Με οδηγό" (ήταν 7).»* Then a deterministic gap-cut into ≤3 access routes (min cluster 3) **rendered as draggable chips the teacher places**. The system never writes an automatic student→route mapping; the record says "teacher assigned X to route B", named actor, timestamp. The unit of generated output is the **class artefact set**, not the student. This is the cheapest possible insulation against an Annex III 3(b) "steering the learning process" finding.

**7 · GENERATE — LLM, fan-out, strict schema, ADDITIVE BY DEFAULT.** One call per (route × card). The model never sees a strategy list to choose from, cannot name a strategy (no field exists), and never sees the class file. Additive ops are the default surface. Rewriting is a per-block "rewrite this paragraph" action the teacher invokes, hard-capped at ≤12 accepted rewrite ops per artefact and rendered as **side-by-side paragraph pairs**, because the unit of judgement for a rewritten Greek paragraph is the paragraph. Routing: `claude-opus-5` for all Greek and all generative work (Greek's ~2.5× token penalty lands on input, which caches at 0.1×, so downgrading the model is the wrong saving); `claude-sonnet-5` for English mechanical; `claude-haiku-4-5` never on the Greek path. Four cache breakpoints, `sort_keys=True`, no timestamp in any cached prefix, and an integration test asserting `usage.cache_read_input_tokens > 0` — that failure is silent and permanent.

**8 · VALIDATE — CODE, blocking, free checks first, one targeted repair.**

| id | check | verdict |
|---|---|---|
| SC-01 | schema + span coverage; no unplanned bid touched | drop op |
| PT-01 | `preserved[]` verified by exact substring — never trusted | repair ×1 |
| CT-01 | command-term rank non-decreasing, **by term identity**, same governed object | blocker |
| CR-01 | `union(strands_after) == union(strands_before)` on any `drop` | blocker |
| CE-01 | top achievement level (7–8) still reachable from the transformed task | blocker |
| TR-01 | tier-3 retention ≥ 0.95 against the subject glossary | blocker |
| NV-01 | novelty gate: numbers ∪ dates ∪ capitalised tokens ⊆ source ∪ glossary | blocker |
| AL-01 | answer leak: no inserted scaffold entails an expected answer (only when a mark scheme exists) | blocker |
| RD-01 | re-measured demand inside the target band — **floor and ceiling**; over-simplification fails | repair ×1 |
| EL-01 | `spylls`+`el_GR` plus minimal-pair guard (πότε/ποτέ, πώς/πως, ή/η, τι/τί) | blocker |
| AG-01 | any Ancient Greek/Latin string is an exact NFC substring of the source | blocker, no repair |
| ST-01 | discourse stems contain no propositional content (no content word outside a closed lexicon) | blocker |
| EG-01 | egress guard on the **outbound** payload: ΑΜΚΑ (11d, DDMMYY prefix), ΑΦΜ (mod-11), ΑΔΤ, phone, email, Greek name-stem gazetteer — **blocks and warns, never silently redacts** | pre-send block |

Repair is one targeted retry with the *specific* violation restated to the *specific* transform. Second failure drops the op and the UI names the barrier now unaddressed. Shipping a failed artefact silently is worse than shipping fewer.

**9 · REVIEW — CODE + BROWSER.** Server-rendered diff. Change cards each quote his span, name the barrier and the named card. `j/k` to walk, `a/r` to decide. Default state pending; accept-all exists and is never the default button. Fidelity badge shows **only when it has news** ("2 ops dropped — command term would have downgraded on Q4", linked to the span). The refused-moves panel is **off** the working screen and **on** the teacher sheet and the export, where the coordinator reads it and the tired teacher does not.

**10 · EXPORT — CODE.** Accepted ops applied to *his* document object at `docx_anchor`, rebuilding the run sequence from a captured formatting template rather than mutating a single run (Word splits one sentence across 5–8 runs on rsid/proofing/`w:lang` boundaries). Two renders from one source: **student copy, zero diagnostic marking, zero route name a student could read as a rank**; **teacher copy** with rationale, evidence, fade dates, refusals. Print pack with correct per-route counts. Art. 50 footer + XMP provenance on **every** copy including the unmodified base — a footer only on adapted copies is itself SEN marking.

**11 · LEDGER — CODE.** Hash-chained append with six version pins. And the outcome signal that actually exists: on every subsequent run, detect **reapplication** (same card, same student-set or class, later document) and compute edit distance on reapply. Human outcome capture is exactly **one prompted item per week**, chosen by the fade-review queue, one tap, three options. No learned prior in v1; outcome data breaks scorer ties only above n≈500 events.
