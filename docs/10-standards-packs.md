# Two standards packs: MYP and Γυμνάσιο

The same text, the same barrier, the same student — and a *different legality verdict*,
because the two systems constrain different things. This is why `standards_pack` is a
first-class key in the verdict table and not a localisation setting.

## The load-bearing difference

| | **MYP** | **Γυμνάσιο (ΕΠΣ / ΙΕΠ)** |
|---|---|---|
| What binds you | **Construct validity.** Criterion-referenced, published descriptors, task-specific clarifications. A scaffold that supplies the assessed behaviour invalidates the level. | **Statute.** The ΚΕΔΑΣΥ γνωμάτευση mandates specific arrangements; the school implements them. |
| The dangerous move | Giving the student the thing the criterion asks them to produce | Failing to apply an arrangement the γνωμάτευση requires — or inventing one it does not |
| Assessment granularity | Criteria A–D per subject group, per-strand descriptors, 1–8 | Numeric 1–20, γραπτή + προφορική, weakly specified differentiation guidance |
| Engine that matters | **The refusal engine** | **The arrangement checklist** |
| Who authorises deviation | Inclusion lead + programme coordinator | ΕΔΥ / ΚΕΔΑΣΥ, by protocol number |

**Therefore the app runs in two modes.** In MYP mode it is a construct-validity referee.
In Γυμνάσιο mode it is a differentiated-instruction generator with a statutory-arrangement
reminder. Same analyser, same strategy library, different verdict pack and a different
default posture.

## MYP: the criteria that decide legality

Construct relevance resolves at the **criterion strand**, never at the subject. The same
move flips.

**Individuals & Societies**
- **A — Knowing and understanding.** The construct is terminology and content knowledge.
  *Substituting* a tier-3 term destroys A. *Glossing* it in apposition — keeping the term,
  adding the explanation — preserves it. Most "simplify this text" tools collapse these two
  and silently invalidate A.
- **B — Investigating.** The construct is formulating the research question and the method.
  Supplying the question destroys B. Supplying a *process checklist* does not.
- **C — Communicating.** The construct is structure and source documentation. A writing
  frame that supplies paragraph structure contaminates C — and is perfectly legal when the
  task assesses only A and D. Writing frames are the most context-dependent scaffold there is.
- **D — Critical thinking.** The construct is analysis, source evaluation, interpretation,
  synthesis. A **blank** OPVL grid is a layout aid and legal. A **pre-filled** one — or one
  that names the limitation — destroys D.

**Language and Literature**
- **A — Analysing.** Interpretation and the effect of authorial choices. Any supplied
  interpretation destroys A. This includes a "summary" of what a poem is about.
- **B — Organizing.** Structure of the student's response. Writing frames contaminate B.
- **C — Producing text.** Creative and critical production.
- **D — Using language.** Register, vocabulary range, grammar, spelling. **Spellcheck,
  word banks and sentence-level correction all contaminate D** — and are pure access on
  an I&S task. This single pair is the clearest demonstration that the verdict cannot be
  keyed on the strategy alone.

**Language Acquisition (for the EAL / Greek-as-L2 cohort)**
- A Listening, B Reading, C Speaking, D Writing. Comprehension support on a Reading task
  is the construct. The same support on an I&S task is access. Phase level determines the
  ceiling, so an "extension" here means a higher phase descriptor, not more questions.

**Always true in MYP:** the command term and the level 7–8 descriptor must stay reachable.
A differentiated task that caps a student at level 5 is not differentiated, it is a
different task. This is a blocking check, not a warning.

## Γυμνάσιο: the arrangements that decide compliance

The γνωμάτευση is a statutory determination, not a suggestion. Typical outputs:
προφορική εξέταση, παράλληλη στήριξη, τμήμα ένταξης, ΕΒΠ/ΕΕΠ support, επιπλέον χρόνος.

**What the app does:** the teacher ticks which arrangements are in force this session.
The app renders them on the teacher copy as a checklist and constrains its own suggestions
accordingly (it does not propose a written extended response when written production is
the arranged exemption).

**What the app must never do:**
- Suggest, infer or recommend an arrangement. Only a ΚΕΔΑΣΥ/ΕΔΥ determination creates one.
- Infer a category, a support tier, or a need for referral from student work.
- Store the protocol number, the date, or anything else from the document.

**One thing worth saying out loud to Greek colleagues.** Προφορική εξέταση is granted almost
reflexively. It is correct for transcription difficulty and for Greek orthographic
impairment. It is **actively harmful** for word-retrieval difficulty and for
social-evaluative anxiety, where it converts a writing barrier into a speaking barrier in
front of an audience. The app should surface that contraindication when the teacher ticks
the combination — it is the single most useful thing it can tell a Greek secondary teacher,
and no existing tool says it.

## The bilingual problem is not translation

Greek and English are separate barrier vectors for the same child. `el:decode.rate=3` and
`en:decode.rate=0` are both true, routinely. Consequences:

- Greek is **shallow for reading, deep for spelling**. The dyslexia signature in Greek is
  *rate* plus *inflectional-ending errors* (-ει / -η / -οι, ο/ω), never accuracy. Any tool
  ported from English screens for the wrong thing.
- Flesch–Kincaid and Lexile are invalid for Greek: inflectional morphology inflates syllable
  and word counts without inflating difficulty. Ship **no Greek readability number.** Report
  the driving features with the offending span quoted — genitive-chain depth, nominalisation
  density, λόγιες μετοχές, subordination depth.
- Greek command terms are not imperatives. They are periphrastic «Να + subjunctive»
  (`Να αξιολογήσετε`, `Να τεκμηριώσετε`) plus bare nominals (`Αιτιολόγηση`, `Σχολιασμός`).
  An extractor that matches imperatives finds nothing.
- In an international school the **L2 cohort is usually larger than the SEN cohort.** If
  that holds at IMS, bilingual glossing and register scaffolding is a bigger v1 lever than
  any SEN scaffold, and the product framing is *academic language access*, not
  *special education*.
