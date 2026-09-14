# Diodos — ranked backlog

Synthesis of six audit passes over README.md + docs/00-14 (16 files, 1,357 lines, 4 commits, zero application code, zero data assets).

**Owner key:** **D** = your judgement, nobody else can supply it · **C→D** = Claude drafts, you verify/cull · **C** = code or drafting, effectively free · **M** = mechanical.
**Size key:** minutes · hours · evening · week. Read every "day" in docs/04 as three evenings.

---

## START HERE

**Tonight.** `mkdir -p fixtures/real/`, copy in 20 of your own `.docx` — ≥4 bilingual EN-with-Greek-margin, ≥3 you know are messy, ≥5 authored by *colleagues in Word* rather than exported from Google Docs, spread across History-MYP-EN / Λογοτεχνία-EL / Νεοελληνική-EL. Write `scripts/survey.ts`. Commit `docs/15-fixture-survey.md`.

That table is the only measurement of your actual document population that exists anywhere, and it gates the next four evenings. It also answers a question nobody asked: if >50% of your documents put assessment content inside `w:txbxContent` / `w:sdt` / OMML, the named-refusal list at `docs/02:9` is not graceful degradation, it is *"this tool cannot read your worksheets"*, and ingest inverts to paste-first before a line of the analyser is written.

**Then, in this order, one evening each:** no-op round-trip → projection invertibility → annotate+split in Word → insert_after + footer → both baselines. Five evenings, and three of them can kill the project.

---

## Three things the audit settled that you may still think are open

1. **The runtime is not an open fork.** `docs/12` is a later commit than `docs/03` and supersedes it explicitly: *"Use JSZip + the browser's own XML parser instead."* What is open is four Python dependencies, and only one of them (`spylls` for EL-01) is blocking. Stop re-litigating the stack; decide the dependencies.
2. **`git log` settles every supersession argument mechanically.** `docs/00-08` have not been touched since `6316da0`. Zero-retention (`73a6ea6`), site architecture (`3608b47`) and the L1-EL/L2-EN correction (`99eb682`) were all written as *new files* — nothing upstream was edited to match. The rule is: **09 > 00-08 on storage; 12 > 00-08 on stack and trust boundary; 13 > 10/11/00 on cohort; 14 > 03/04 on layout.** Nine of fifteen docs are pre-supersession originals.
3. **This is a content product wearing a software costume.** Strip the seven operations, the JSZip round-trip and the React shell and what remains — ontology, cards, verdicts, two command-term lexicons, bridge pack, glossaries, Greek rule sets, refusal strings — is ~4,500 authored rows, ~700 of them irreducible φιλόλογος/MYP judgement. `docs/04` budgets three evenings for the two largest blocks of it. Realistic: **~130-160 hours, ~60 of them before first classroom use.**

---

## TIER 0 — blocks everything, this week

| # | Item | Owner | Size | Falsifies / unblocks |
|---|---|---|---|---|
| 0.1 | **20-document fixture corpus + `docs/15-fixture-survey.md`** — stratified, not 20 exports of one template. `docs/04:4` requires 20 real worksheets; zero exist. | **D** | evening | Ingest scope, the refusal list, four later decisions |
| 0.2 | **`engine/docx/pkg.ts` + the no-op canonical test, WITH a pass condition** — `README.md:22` gates the whole project on this passing and `docs/04` states no threshold for any of its three tests. Write `canonical()` first (strip `w:rsid*`, `w14:paraId/textId`; drop `w:proofErr`/`w:noProof` **nodes** — `docs/04:4`'s "proofing attributes" is a category error, they are sibling elements; sort attributes by `(namespaceURI, localName)`; normalise empty-element form; text byte-exact incl. `xml:space`). **Pass = ≥17/20.** Verify in Word, Win **and** Mac; never LibreOffice, which is more forgiving and gives false passes. | C+**D** | evening | *"It gives you back your own file"* — the entire wedge |
| 0.3 | **`project()`/`unproject()` with dual raw+NFC strings, tested on ~1,500 real paragraphs** — **THE RISKIEST ASSUMPTION AND NOBODY SCHEDULED IT.** Every op in `docs/01:96-103`, PT-01, AG-01 and the O(changes) thesis address a flat string the document does not contain. `docs/02:11` mandates NFC while the XML may hold NFD; Greek with accents differs on nearly every content word. Keep **two** strings per paragraph — `raw` (sole SpanEdit offset domain) and `nfc` (analyser domain) — plus an index map. Exclude `w:delText` and the `mc:Fallback` branch or every later offset shifts. | C | evening | That character ranges are a usable address space over bilingual Greek Word documents. **Not green → no feature work. This is a stop.** |
| 0.4 | **`annotate` + `split` applied for real; open all 20 in Word, Win and Mac; repair-dialog count must be 0** | C+**D** | evening | In-place mutation surviving Word. First screenshot you can show a colleague: your Παπαδιαμάντης in κῶλα, your fonts intact |
| 0.5 | **`insert_after` + the Art. 50 footer + `Node`/`RenderDirectives` defined** — clone existing nodes, never `createElementNS` (or every insert carries a redundant `xmlns:w`); strip `w:numPr` from clones or a glossary box after Q3 renumbers Q4→Q5 across the whole sheet, silently, on paper; direct-formatted `w:tblPr/w:tblBorders`, never injected style definitions; `xml:space="preserve"` on any `w:t` with an edge space; `DOCX-ORDER-01` post-write assertion. Footer = new part + `[Content_Types].xml` Override + relationship + `w:footerReference` on **every** `w:sectPr` including `first`/`even` under `w:titlePg`. `docs/04` does not mention the footer at all, and it contradicts "untouched", which changes what 0.2 is asserting. | C | evening | Additive ops actually being additive. Also settles: **OOXML has no word-spacing property and no measure control short of page margins, so the print pack — not the .docx — is the faithful render for render directives.** No doc says this. |
| 0.6 | **Both baselines in one evening, with the decision rule written BEFORE the run** — 12 golden items (6 EL, 6 EN), naive single prompt, downgrade rate / tier-3 retention / profile-swap Jaccard; then stopwatch 10 real tasks by hand with **two fixed tick-sets written down in advance**. Commit `docs/16-baseline.md`. Note: profile-swap Jaccard is **not computable** on a single-prompt baseline — `docs/06:113` and `docs/08:14` both define it over strategy *sets* and prose has none; redefine as edit-span Jaccard or accept a hole. The 5 archetype profiles also do not exist. | **D** | evening | *"The app beats the chatbot you already have open."* `docs/00:150`: "the entire business case and it will never be this cheap to obtain again." `docs/00:184`: "Nobody else can produce this number." |

**Kill conditions, with numbers, from these six:**
- 0.2 below 17/20 → in-place mutation is dead. Fall back to "annotate a read-only HTML render, export a companion sheet." That is a different product with a weaker claim — say so rather than patching. Prepare the stronger fallback in the same session: never re-serialise `document.xml`; locate each `<w:p>` by byte range and splice replacement XML as raw text, which makes the assertion *trivially* true instead of *hopefully* true.
- 0.1 shows >50% of documents with assessment content in text boxes/SDT/OMML → ingest inverts to paste-first.
- 0.3 not exactly invertible → stop until green.
- 0.6 baseline already <10% downgrades and >0.9 retention → the refusal engine's measured value is small; re-scope to the bridge pack and the command-term card before building the verdict engine.
- No real classroom use by end of week 3 → cut the standards pack and refusal engine from v1 and ship the bilingual command-term card alone (`docs/00:151`: "If it slips past week 4, something in the scope is wrong, not the schedule").
- Median drop→print above **40%** of the 0.6 manual median → the saving is not there. *This kill condition exists in no current doc, and `docs/00:184` demands the number specifically so it can.*

---

## TIER 1 — decisions only you can make, ranked by design surface controlled

Each costs minutes to hours. Each blocks work that costs days.

**1.1 — Name the one artefact you make every week.** Subject, language, print/digital, adapt/generate. One sentence. `docs/00:180` asks it; nothing answers it. It picks which analyser gets built first — `docs/04:5` commits the highest-ROI day to the Greek triad, `docs/13:68` says bilingual command-term unpacking is *"the highest-yield single feature for this cohort and it costs zero model tokens"*, and those are different week-1 builds. It also picks which of the three tables in `docs/11` supplies the 15 seed verdict rows. **Default:** MYP I&S History, English source with Greek margin, printed, adapt only. **D · minutes.**

**1.2 — Does v1 have a server, and who pays?** `docs/12:7` no account · `docs/12:58` one server route · `docs/03:14` €0.20-0.45/worksheet · `docs/14:58` *"Anyone who sees a link can click it."* As specified this is a free unauthenticated Opus proxy funded by you. Zero occurrences of `rate limit`, `quota`, `captcha`, `abuse` or `budget` in 1,357 lines. **Default:** no server in v1 — deterministic-only, client-side, generative unlocked by a BYO key in IndexedDB. That deletes cost, abuse, EU-region and provider-terms work in one move and makes `docs/12:46` unconditional. **Hedge if the habit forms on generative moves:** one route, demo doc only, hard per-IP daily cap, provider-level budget action that disables the key. **D · hours.**

**1.3 — Where does `assessed_verb` come from on the adapt path?** It is in the verdict PK (`docs/01:50,55`) and the lookup (`docs/02:19`). DECLARE collects mode/subject/criteria in three clicks (`docs/02:15`); screen 1 collects mode/subject (`docs/12:96`); **only the generate path collects a command term** (`docs/12:77`). So on the adapt path — the default, the 22:40 case — every lookup misses on a key component the UI never obtained and falls to the default, which is `invalidating` on summative. **The refusal engine as specified fails closed on 100% of summative adapts** — precisely the *"inert outside the corridor he hand-built"* failure `docs/00:58` warns against. Also pick one key: `docs/01:55` vs `docs/09:43` (which adds `criterion`, drops `assessed_verb`) disagree, and `assessed_verb` is load-bearing per `docs/06:19`. **Default:** derive from the command term the extractor already found, confirm on ≤4 items, make `docs/09:43` match `docs/01:55`. **D · minutes.**

**1.4 — The four Python dependencies.** `spylls`+`el_GR` (EL-01, blocking), `textstat`, `wordfreq`, spaCy `el_core_news_md`, plus Morpheus/CLTK if ΑΕ is in scope. **The clean resolution nobody wrote down:** EL-01 only ever inspects *generated* text — source characters are never retyped (`docs/11:123`) — and generated text has by definition already left the device. **Run EL-01 server-side on generated spans only. Zero new egress, zero payload cost,** minimal-pair guard stays client-side as a ~2KB table. `textstat`→banded heuristic; `wordfreq`→shipped top-50k list; spaCy cut from v1. Then amend `docs/12:46` to name the one exception out loud rather than leaving the sentence false. **D · evening.**

**1.5 — Print or Classroom, and fix the kill predicate.** `README.md:56` and `docs/04:20` measure the only external signal as *"exports a printed pack in week 2 unprompted."* If IMS is Classroom-digital that returns a false negative on a tool that works, and `docs/00:121` makes that stop-building. **Change the predicate to "exports or assigns any pack" now, regardless of the answer.** One line. **D · minutes.**

**1.6 — Where does URW op-history live?** `README.md:53` governs the project on it. `docs/09:37-40` fixed the key (`strategy_card × typed class_handle`) but not the store: no ops table, `localStorage` restricted to "preferences and glossaries", no login means no cross-device continuity. The week-8 test is worse — two teachers, no accounts, *"No analytics that record content"* (`docs/12:60`), no counts schema, so the only channel is asking them, which contaminates *"no demo, no onboarding, no support."* **Default:** random `installation_id` in IndexedDB (disclosed in one line, cleared by one button) + op log keyed by typed `class_handle` + a salted hash of the op payload for "unedited" — which also makes the 65-85% accept band, `demand_restore` and `refusal_rate_per_artefact` computable at all. **And amend `README.md` to say the week-8 result is self-reported at n=2**, rather than leaving a kill rule the architecture cannot feed. **D · hours.**

**1.7 — The IB copyright line for command terms.** `docs/06:19` raises derivative-work exposure for strand descriptors only, and the adopted fix was designed for a private SQLite table on a laptop. IB also publishes command-term definitions, and the obvious way to fill 60 `examiner_wants` lines is to paraphrase them. `docs/14:65` then proposes shipping packs publicly. **Default:** store the term string (a word, not protectable) plus your own examiner line, written from what you tell students. The `trap` field is original by construction and is the valuable half. **Write the rule into `docs/10` before authoring a single row** — it blocks 95 rows. **D · hours.**

**1.8 — Route clustering under zero retention.** `docs/02:21` and `docs/00:76` specify a gap-cut into ≤3 routes (min cluster 3) plus «3 μαθητές χρειάζονται ακόμη τη διαδρομή "Με οδηγό" (ήταν 7)». Both need per-student vectors. `docs/09:26` deletes them; `docs/12:103` collects room-level ticks. **There is nothing to cluster and nothing to count 7→3 with.** **Default:** anonymous counts per barrier combination — no roster, same shape as the free-text line already accepted. Or drop the payoff number and say so. Blocks pipeline step 6 and the class-wide-uplift-before-overlay split `docs/00:139` says kills adoption in week three if wrong. **D · hours.**

**1.9 — Do MYP and Γυμνάσιο ever apply to the same artefact?** `docs/12:96` makes the toggle exclusive; `docs/01:55` carries one `standards_pack`. MYP4 ≈ Γ′ Γυμνασίου. If one document serves both registrations the lookup silently returns the verdict for the *unselected* pack — the one place the refusal engine can be confidently and silently wrong. **Default:** exclusive, and print the declared pack on the teacher copy so a wrong toggle is visible. **D · minutes.**

**1.10 — Does `criteria_modification` survive with no login?** Gated "to coordinator level" in four places (`docs/00:62`, `docs/05:15`, `docs/06:77`, `docs/07:23`) against `docs/12:5` *"No login in v1."* No identity, no role model. **Default: cut it.** A fourth verdict value with an unenforceable gate is worse than its absence. If kept, reframe as a typed institutional declaration plus the printed eligibility consequence, and say it is an honesty mechanism, not an authorisation one. **D · minutes.**

**1.11 — Ancient Greek in scope? Γυμνάσιο or Λύκειο?** `docs/11:3` scopes three subjects; `docs/00:110`, `docs/02:39` (AG-01, blocking, no repair), `docs/04:14` and `docs/11:64-67` specify a whole ΑΕ subsystem, and `docs/01:107` already carries a `grc` tag. Separately `docs/07:21` mentions πανελλαδικές (Λύκειο) while the pack is `gymnasio-el.yaml` — and `docs/14:33-37`'s "validates itself on day one, for free" is only true if the Γυμνάσιο rows come from tasks you set. **Default:** AG-01 and colometry stay (substring check, index splits, no generation); Morpheus/CLTK cut; Γυμνάσιο schema built with **zero** seeded rows until you name a real task. **D · minutes.**

**1.12 — Licence split, domain, repo name.** `docs/14:65` (community packs = the bus-factor answer) vs `docs/00:61` (verdict pack = a subscription). Mutually exclusive, no LICENSE file. Repo is `/home/user/Special-Education` while `docs/11:112` says *"the product is called something other than a special-education tool."* `docs/14:58` makes a public link the entire go-to-market and no domain is named anywhere. **Default:** engine MIT/source-available, packs CC BY-SA with a required `license` field. `docs/06:7` says the asset is *authority*, which publication maximises. Buy the Latin domain this week, keep Δίοδος as wordmark, rename the repo. **D · hours.**

---

## TIER 2 — content assets, the real shape of the project

Total **~130-160 hours**, ~60 of them before a student sees paper. At 8-10 usable hours a week that is **four months**, not `docs/04`'s "Day 2, Days 4-5". Restate those days as what they are, or the schedule lies to you in week one.

### On the critical path to first classroom use (~60h)

| Asset | Count | Owner | Effort | Note |
|---|---|---|---|---|
| Barrier ontology completion | 19 codes × (ICF-CY + 2 observables + 3 anchors + flags) | C→**D** | 6-8h | The 19 exist at `docs/01:72-79`. Zero ICF-CY refs, zero observable questions — and they must be bilingual because they are the chip labels on screen 3. Also missing: which codes are language-bound (`:43` mandates `el:`/`en:`/`grc:` prefixes, the example prefixes `el:decode.rate` and leaves `wm.verbal` bare, no per-code flag exists, so **no parser can validate a line**), and what severity 1\|2\|3 means when screen 3 *ticks* (a tick is binary). Delete the six conflicting sizes in `docs/07`/`docs/08`. |
| **Feature → demand-code → load mapping** | 3 thresholds × ~8 features × year bands | **D** | week | **THE LARGEST HOLE IN THE REPO.** COLLIDE compares `demand[code].load >= s.min_load` and nothing states how «ονοματοποίηση 7,2/100» becomes `syntax.complexity=2`. No candidate set, no plan, no product. |
| Greek analyser rule data | ~15 suffixes, ~10 participle patterns, ~120 connectives, ~200 syllabifier exceptions | C→**D** | 10-12h | `-ων` is claimed by the genitive-plural **and** the λόγιο-participle detector simultaneously — they fight on every «των ανθρώπων». Nominalisation needs a denominator, a completed suffix list and a stop-list (πόλη, γνώση, κατάσταση). υπερβατό is undefined entirely. Polysyllable ratio has no consumer since `docs/00:106` ships no band. Restate the syllabifier oracle: `hyph_el_GR` is a Liang **hyphenation** file; hyphenation points ≠ syllable boundaries, so the ≥99% is measured against the wrong thing on an unspecified word list. |
| EN command terms | ~60 | **D** (glosses C→D) | 8-10h | rank / aliases / el_gloss / el_pair / `examiner_wants` in your words / `trap` / strands. The `trap` on *to what extent* is the asset: `docs/13:56-60`, a Criterion D mark lost to a comprehension failure in the stem. Rank scale is undefined and CT-01 matches by **identity**, so decide what rank is for. |
| EL command terms | ~35 | **D** | 6-8h | «Να + subjunctive» + bare nominals as headings (Αιτιολόγηση, Σχολιασμός). `docs/00:112`: *"Any extractor that matches imperatives finds nothing."* Γυμνάσιο/ΙΕΠ terms are **not** the same set as IB's Greek renderings — two lists. Patterns tested against real worksheets, not written blind. |
| Cross-language rank alignment | ~35 | **D** | 3h | **Named in no document.** CT-01 is "by term identity" and `docs/13:79` makes bilingual the default render; on independent scales CT-01 cannot compare and a gloss silently downgrades across languages. CT-01 is decorative without it. |
| Strategy card schema + the ten zero-token cards | 1 schema + 10 cards | C (schema) / **D** (cards) | evening + week | Schema fields scattered across five docs, never collected; `execution_kind` **never enumerated** yet `docs/01:48` makes a load-time invariant depend on it; contraindication graph has exactly one known edge. And **the ten are never named**: `docs/11:118` says seven operations, `:129` says "ten of these cost zero model tokens." Ten of seven. Quick Mode — *"the first thing that works"* — has no manifest. |
| Organiser templates + render presets | 10 + 6 | **D** | 7h | Hardest: `docs/11:51` makes marginal gloss the top Literature move, but a docx margin gloss is a text box / comment / footnote / two-column table — **and text boxes are on the ingest refusal list.** Same unresolved for timeline strip, actor card, OPVL grid, A6 card (4-up on A4 with cut guides, or the "viral artefact" is a teacher with scissors). |
| Named refusal strings | ~30, EL+EN | **D** | 4h | `README.md:18`: *"That sentence is the product."* `show_on` defaults to `[teacher_sheet, export]`, never the working screen (`docs/06:67-69`). Includes the `summative_external` hard-stop content. |
| Tier-3 glossary, one unit | ~40 | M+**D** | 2h tooling | TR-01 is **blocking** against "the subject glossary" and the glossaries do not exist. |
| Demo documents | 3 | **D** | 4-6h | (a) ~280-word Greek history source written *deliberately* to genitive depth 4 and nominalisation ≥7/100 so screen 2 has something to find — doubles as golden item #1; (b) EN MYP I&S source-analysis with a Diii item and a *to what extent* stem so refusal **and** trap fire on first click; (c) public-domain literature (Παπαδιαμάντης d.1911, Σολωμός d.1857). **Not** an ΙΤΥΕ extract — `docs/06:71` flags that licensing unexamined. |
| UI strings | ~250 × 2 | C→**D** | 4h | |

### On the critical path to the product's actual claim (~15h more)

| Asset | Count | Owner | Effort | Note |
|---|---|---|---|---|
| Verdict seed rows | 15 | **D** | 5h | **The authoring instruction that prevents the wrong 15, written nowhere:** open your next *three real summative tasks*, run COLLIDE, author a row for every card it proposes. Fifteen rows at 100% hit rate on your own work, not fifteen at 5%. `docs/06:55` already predicts the alternative: *"month one reads as 'the tool refused most of what I asked'."* |
| MYP standards pack | ~40 strands | **D** | 3h | Identifiers only. **Pick a canonical strand form first** — the docs write `Diii`, `D-ii`, `Ai` and `A–D` for the same thing. |
| Γυμνάσιο pack + arrangement contraindications | ~12 + ~8 | **D** | 3-4h | The προφορική-εξέταση contraindication is what `docs/10:80` calls *"the single most useful thing it can tell a Greek secondary teacher, and no existing tool says it."* |
| Evidence grades | **~85 (card × barrier) pairs** | C→**D** | 12-15h | `docs/00:137` requires per-pair, not per-card. **Nobody did the multiplication** and it appears in no plan. It is what stops the product being the "evidence-based strategy" laundering its own brief condemns. |

### Grows from use — start empty by design

Verdicts beyond the seed (`verdict_candidates` **is** the authoring queue — `docs/02:19`) · tier-3 glossaries per unit · entity lists per unit · cognates beyond the top 150 · golden set 12→24 · false friends (start at the 15 you've observed, grow every time you mark a set).

### Start empty but must NOT be allowed to

**Evidence grades** and **the ST-01 closed lexicon** (EN ~300, EL ~350 function words). Both have blocking gates pointing at them. `docs/02:40` makes ST-01 a blocker — *"no content word outside a closed lexicon"* — and that lexicon is named in no document, with no shape, no count, no path and no owner. **Empty, ST-01 ships as `return True` and nobody notices**, which is worse than not having the validator.

### Bridge pack (parallel, highest value-per-hour in the project)

False friends 40-60 (**D** culls, C drafts, 6h — the `observed` flag is the discriminator) · cognates 200-250 (M from AWL ∩ Greco-Latinate, 4h + 3h of your judgement on the `transparency: partial` warn lines — *synthesis/σύνθεση* means "a composition" in a Greek school, so a student told to synthesise writes an essay *about* the sources) · transfer errors 20-30 (**D**, 2h) · tier-2/AWL ~570 families with ~180 connective/hedge/stance flagged (M + glosses, 3h).

---

## TIER 3 — architectural gaps that must be settled before the matching code is written

**3.1 — COLLIDE is written wrong for the declared cohort.** `docs/02:17` keys on `code` alone; `docs/01:43` and `docs/13:80-84` insist profiles are per-language (*"`el:decode.rate=3 · en:decode.rate=1` is not a contradiction — it is the expected pattern"*) and `Block` carries `lang`. Must be `severity[lang][code]` against `demand[lang][code]` per block, or an English barrier fires moves on Greek paragraphs. **This is the core matching rule of the product.** C · hours.

**3.2 — Make the four undecidable validators decidable.** `CE-01` ("top level 7-8 still reachable") is asserted blocking in five places and has **no algorithm anywhere**; deciding it needs the level-7-8 descriptor, which `docs/01:48` forbids storing. Reduce it to something checkable (`CT-01 ∧ CR-01 ∧ no response-length/format cap`) or it becomes exactly the gauge that always reads full that `docs/06:67` warns about. `RD-01` needs a "target band" that `docs/00:106` forbids shipping — restate as per-feature floor/ceiling deltas. `EL-01`'s minimal-pair guard is undecidable by a spellchecker (both spellings are valid words; choosing needs syntax) — restrict to tokens the app itself inserted, or demote to a review flag. `ST-01` needs the lexicon above. `AL-01` needs a mark-scheme ingest path that does not exist. **And nothing enforces the Literature absolute** (`docs/11:42-44`, *"the only absolute in the system"*) — which is also not expressible, since the CHECK allows four values and `docs/02:19` gives **every** refusal an override control. C→**D** · evening.

**3.3 — The client-side EG-01 egress guard.** `docs/07:16` and `docs/04:26` implement it with Presidio, which is Python. Under `docs/12:40-42` it must run in the browser *before* the fetch. This guard got **more** important under the site architecture — spans of the worksheet now cross a network, and `docs/05:7` names the worksheet as the real leak vector — and simultaneously lost its implementation. Needs the Greek name-stem gazetteer (~9 suffixes + ~800 given names, licence-checked) and an ΑΔΤ format spec, neither of which exists. **Nothing should hit the server before this exists.** C+M · hours.

**3.4 — Screen 3's free-text line breaks the trust boundary printed next to it.** `docs/12:103` accepts «3 αργοί αναγνώστες, 2 νέοι στα ελληνικά»; `docs/12:25`, same page, says *"The server never receives a barrier code, a group label, or anything describing a person"* — and that sentence describes persons. Either a client-side Greek/English keyword→code table (brittle, say so) or an LLM call the same page forbids. **D** · minutes.

**3.5 — The student-copy sanitiser, distinct from EG-01.** `docs/02:47` promises *"student copy, zero diagnostic marking"* and `docs/00:80` says *"Dignity is a product requirement"*, but the only egress control specified guards the **model payload**, not the **document**. An in-place edit that preserves everything untouched — the entire premise — preserves `word/comments.xml` ("Μ. δυσκολεύεται εδώ"), last year's `w:ins`/`w:del`, `w:rsid` author history and the core properties straight into the copy handed to the student. **And the filename is mentioned in no document and is the most-read string in the artefact:** `9B-istoria-guided.docx` on a shared drive is SEN marking by filename, after all the care taken over footers and route names. Strip comments and revision markup, reset properties, emit `-A`/`-B`/`-C`, show what was stripped on Export. C · evening.

**3.6 — The chip board has no keyboard path, and it is the Annex III mitigation.** `docs/02:21` renders routes as draggable chips; `docs/05:9` makes that drag the 3(b) insulation. **A teacher who cannot drag cannot use the feature that makes the product legal.** Same data model, zero extra cost now, a rewrite of the regulator-facing screen later. C · hours.

---

## TIER 4 — subsystems nobody has specified

Ranked by cost-of-discovering-late.

1. **Rate limit, spend cap, kill switch** — cannot be retrofitted after a link spreads. Provider budget action → app-level daily EUR counter that disables *only* generative moves and leaves the client-side path working (say that on screen; no other product degrades this gracefully) → edge token bucket ~20 runs/IP/day + Turnstile → server-side block cap. And name the defence you already have: `SpanEdit[]` against spans of the user's own text under `additionalProperties:false` makes the endpoint nearly worthless as a general LLM. That is a security property; put it in `docs/12`'s table. C · evening.
2. **Session recovery + IndexedDB** — `docs/09:14` forbids disk for barrier profiles; `docs/12` specifies 10-30 minutes of skilled work held in a JS heap with no mention of refresh, sleep or tab discard. Persist the document, analysis, declared mode and op decisions; deliberately drop the ticks; show "re-tick the room", already priced at 15 seconds. It makes the zero-retention claim *stronger*. Also: `localStorage` is ~5MB, synchronous, string-only, against 2-20MB worksheets. C · evening.
3. **Input bounds + a named refusal for scans** — no floor, no ceiling, no selection. 40 pages × 3 routes = hundreds of calls and 900 change cards; a 3-word paste prints «ονοματοποίηση 33,3/100» on the trust screen. ~120 blocks / ~8,000 words with a page picker; ~40-word floor. Scans have no `<w:p>` anchors, so no round-trip and no wedge — refuse them in the app's own voice, on the landing page. C · evening.
4. **Generation UX** — one sentence in the entire corpus (`docs/07:92`). Deterministic moves render instantly, generative cards stream into the same list with per-card skeletons and per-card error rows, 90-second budget, cancel. Decide client-orchestrated fan-out vs a queue, because 15-40 calls through one serverless route exceeds the execution limit. C · evening.
5. **Print CSS** — `print-color-adjust: exact` (without it the two highest-yield moves in `docs/11` print **white** on the artefact that *is* the deliverable), `@page { size: A4 }`, `break-inside: avoid`, one print view per route with a "Print route B (×3)" button, and the Art. 50 footer tested in print. C · evening.
6. **Pack schemas + `validate-packs` + referential integrity** — `docs/14`'s pack subsystem is an ASCII tree plus *"Neither is engine work."* Every card targeting a merged ontology code silently never fires, and `docs/06:55` establishes false refusals are *"invisible by construction, uncorrectable."* One JSON Schema per pack type + a CI script asserting every cross-pack reference. Bundle at build time so the offline claim stays true. C · evening.
7. **Deterministic test layer** — the character-preservation property test (an afternoon, guards the entire premise), positive **and negative** fixtures for all 13 validators (a blocker with no negative fixture silently stops firing), the committed horrible-docx corpus, the syllabifier word list, EL command-term surface-form tests. Move the generative eval out of pre-commit — a hook costing €2-4 and four minutes gets `--no-verify`'d in week two. And sanitise the golden set on entry: it is made of your real worksheets. C · week.
8. **Content-free error telemetry** — `docs/03:11` refuses observability because "he is the only user", which expires the moment it is a public site. `{stage, error_class, byte_size, docx_feature_flags, browser}` + a "send the *shape* of this file" button behind an explicit JSON preview. That fingerprint corpus is the highest-value dataset available and contains nothing about anyone. C · hours.
9. **Empty/error/offline states for all five screens** — including the Plan screen with zero surviving moves, which is the **designed** week-one outcome. C · evening.
10. **Accessibility** — non-colour channel + a plain linear findings list on screen 2 (screen-reader usable, print-safe, better at 22:40); per-span `lang` (functional, not decorative — without it a screen reader reads Greek in an English voice and wrong `w:lang` red-underlines the whole student copy in Word); scope `j/k/a/r` to the list; axe-core in CI + a keyboard-only walkthrough per screen. Still no conformance claim. C · evening.
11. **Undo** — three grep hits for "undo" in 1,357 lines, all the word "undocumented". `a` and `r` are adjacent keys; `ops.decision` is already nullable. One hour. C · minutes.
12. **Browser floor + memory ceiling + one LibreOffice check** — zero grep hits for Safari/Firefox/Chrome. `showSaveFilePicker` is Chromium-only; a 20MB docx is ~200MB of JS heap. C · hours.
13. **Prompt assembly, cache breakpoints, key handling** — `docs/00:100` calls the cache failure *"silent, permanent, and 3× on the bill"*, which makes the `cache_read_input_tokens > 0` assertion the highest-value single test in the corpus, and it currently has no runtime to live in. Per-card rules blocks mean per-card cache entries; the four breakpoints are unplaced. Zero hits for `secret`: server-only, never `NEXT_PUBLIC_`, rotation, and the fact that the unauthenticated POST route **is** the key. C · hours.

---

## TIER 5 — documentation to delete or rewrite

Blocks nothing technically. Blocks everything practically, because anyone reading front to back — including you in three weeks — builds the dead version.

- **`docs/03-stack.md` — delete and rewrite.** All 14 lines stale. README still indexes it as current.
- **`docs/04-build-sequence.md` — rewrite.** Predates every scope decision since commit 1. No week for standards packs, bridge pack, command-term unpacking, the generate path, the packs refactor, the screens, the demo document or the Γυμνάσιο checklist. Fix the day-3/day-5 circularity: author `id` + `selects_on.barriers` for all 28 cards as a two-column list (30 min), test on that, freeze, *then* write full cards — because rewriting 28 cards' `selects_on` after a late merge is the expensive version of this mistake. And note the test that actually binds is the 15-second chip stopwatch (`docs/09:32`), not the paper exercise.
- **`docs/01-data-model.md`** — delete `classes/9B-istoria.class` (initials, dated evidence, a ΚΕΔΑΣΥ arrangement with an expiry, persisted to git — the exact artefact `docs/09` exists to abolish, and the one thing a DPO would quote back at you) and all the SQL. **Keep `verdicts`**, as a pack schema. Reimplement the parser's one good rule — *"`+ strengths` is mandatory or the line fails to parse"* — as a UI gate: Plan does not unlock until one strength chip is set.
- **`docs/02-pipeline.md`** — drop step 11, renumber 0-10; JSZip not python-docx; spec-seeded not topic-only; **client-side diff** (a server-rendered diff uploads the whole document and voids `docs/12:22` and `:46`); delete "record this observation" (nowhere to record to). Add headers/footers, footnotes and pre-existing `w:ins`/`w:del` to the refusal list, and record the flat-projection contract + the raw/NFC rule.
- **`docs/07-constraints.md`** — ~18 dead lines. Two deserve singling out: `:96` progressive profiling is not stale, it is **the prohibited feature** (deriving a barrier vector from drag history is inference-as-profiling, which `docs/05:9` forbids and `:18` says is always high-risk with no derogation); and `:17`'s "Law 3699/2008 statutory categories" in the controlled vocabulary instructs a future author to build the one asset the product exists to refuse.
- **`docs/11` §Greek-as-L2** — retracted by `docs/13:3-5`, still says *"it should drive v1."* Same residue at `README.md:74`, `docs/10:51`, `docs/10:98-101`, `docs/00:181`.
- **`docs/05`/`docs/09`** — the "software on the teacher's own device ... no processing by a third party" sentence is false the moment a hosted route receives spans. Pick: minimal processor posture (two paragraphs, reinstates a slim DPA), fully client-side/BYO-key (claim intact), or keep it and accept it fails the first DPO who reads `docs/12:22`.
- **`README.md`/`docs/00`** — "verdict ledger" names the dead thing (`README.md:48`, same page); "zero commits" (there are four); three dangling references (`data_model_sketch`, `pipeline`, `build_sequence`).
- **`docs/06`/`docs/08`** — archival headers. `docs/08:46-50` ranks moat 22 above wedge 21, and `docs/09` then chose the most extreme *wedge* variant — the one `docs/08:44` calls *"a very good Greek measurement instrument plus a docx diff"* — without the scorecard ever being amended. Also `docs/08:5-6` is an empty table and `docs/06:67` still says 11 validators; it is 13.

---

## TIER 6 — parallel today, independent of everything above

- **The reference document, published week 4 regardless of build state.** `docs/00:163` and `docs/06:7` both recommend it; neither schedules it; `README.md:48` treats it only as the consolation prize. It is the **only uncontaminated external signal available before week 8** — `docs/06:11`: every other signal is *"structurally contaminated by his own authorship."* Three weekends, and the week-1 weekend already builds half of it. **D.**
- The bridge pack (false friends + cognates). **C→D.**
- The doc-hygiene pass. **C**, one evening.
- Licensing and naming: IB line, ΙΤΥΕ corpus, dictionary licences, AWL terms, LICENSE file, domain, 20 minutes on EUIPO. **D.**
- The Refusals page (10 graded rows with citations — `docs/00:135` commits to it and `docs/12` lists five screens and no static pages at all). **C→D**, a day.
- The landing page. `docs/14:58` makes the link the go-to-market and there are **zero lines of visitor-facing copy anywhere in the repo.** The privacy sentence is the strongest asset in the project and currently lives only in `docs/09:21-22`. **D**, an evening.

---

## Weeks 5-12

| Week | Work | Falsifies / delivers |
|---|---|---|
| 5 | Use it for every lesson; change only what blocks you. Ship the EN analyser properly — AWL connectives, hedging, stance | `docs/13:13` says the barrier is tier-2, and `docs/04` has no EN analyser work at all |
| 6 | Cognate bridge + false friends in the margin. Bridge pack to 150. **Run the external test EARLY** — it is a URL now, so it costs nothing to run twice | That "no existing tool assumes a specific L1" is a real advantage rather than a nice paragraph |
| 7 | Νεοελληνική pack (περίληψη as the macro-rule card). **Γυμνάσιο standards pack authored properly** | **This, not the first pack, validates `docs/14`'s interface claim.** If the engine needs changing to fit statute-binding rather than construct-binding, the abstraction was wrong — better to know in week 7 than month 9 |
| 8 | External falsification test, formally. Two teachers, a link, a demo doc, no onboarding. Evaluate URW | `docs/00:169`: "the honest prior is that it fails" |
| 9 | Ablation gates in CI: content-ablation Jaccard < 0.5, profile-ablation < 0.45 | The second-use-cliff detector `docs/08:14` names as the wedge's biggest risk |
| 10 | **The rewrite path**, unlocked: ≤12 ops, side-by-side pairs. `docs/04` puts it in week 3 | Deliberately last. `docs/00:26` lists prose fluency under *Not durable*, and `docs/06:35` shows O(changes) collapses to O(document) on exactly the dense Greek that justified the analyser. If the habit has not formed by week 10 it never will, and this is wasted |
| 11 | *Only if week 8 passed:* PDF text-layer ingest, answer-leak gate, library to 40 cards | — |
| 12 | **The fork `docs/04` avoids.** If URW < 2 and the external test failed: three weekends publishing the bilingual verdict reference plus the command-term and false-friend packs | `docs/06:7`: *"If the judgements are not good enough to publish under his own name, they are not good enough to sell to a school either."* A branch with a real artefact, not a failure state — and `docs/04`'s month 3 (Postgres, RLS, OIDC, Presidio, DPA, DPIA, Access Change Log, ΙΚΕ) is deleted outright by `docs/09:5-7`, leaving only the Art. 6(4) memo, which is one afternoon |

---

## What end of week 2 must look like

**The project is alive iff:** you stood in front of a real class with paper that came out of Diodos, produced from one of your own `.docx` files, by a browser tab with the network disabled — **and you did not open the exported file in Word and fix anything before printing.**

That last clause is the whole test. A hand-fix means the round-trip claim is false in practice no matter what the canonical-equality test said, and it is the only version of this milestone that cannot be passed by lowering the bar.

If screen 3 is ugly, if there are four cards instead of ten, if the verdict engine does not exist yet — alive. If the paper came from a template you typed instead of your own document — dead, and what died is the wedge premise.
