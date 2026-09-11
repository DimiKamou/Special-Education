# It is a site

A browser app, not a desktop tool. Three consequences, one of which improves the design.

## 1. No login in v1

Open the URL, paste or drop a file, tick some boxes, download the pack. No account, no
onboarding, no roster, no setup gate. Accounts arrive only when a teacher wants *their own*
glossaries and verdict rows to follow them between devices — and even then they store
nothing about a student.

This is the strongest possible expression of "nothing saved": there is no account to attach
data to.

## 2. The trust boundary, stated precisely

| | Browser (the teacher's device) | Server |
|---|---|---|
| Barrier ticks, group labels | ✅ held in memory, dies with the tab | ❌ never |
| Student names | ✅ never typed — there is no field | ❌ never |
| Diagnostic documents | ❌ no code path anywhere | ❌ no code path anywhere |
| The teacher's document text | ✅ | ⚠️ only the spans a generative move touches |
| Numeric transform parameters (`target_wps`, `chunk_size`, `max_new_vocab`) | ✅ | ✅ |

**The server never receives a barrier code, a group label, or anything describing a person.**
Barrier codes do their work in the browser, during strategy selection, and enter the model
call only as anonymous numbers. A payload leaving the device is: *this paragraph, split to
about 14 words per sentence, keep these terms verbatim.* There is no student in it.

## 3. The improvement: deterministic moves never leave the device

This is the part that gets better by being a site, and it changes a decision in the brief.

The brief specified `python-docx` behind a Python sidecar. **Use JSZip + the browser's own
XML parser instead**, editing `<w:p>` elements in place and re-zipping. One language, and —
decisively — *the same code runs client-side*. Which means:

- **The ten zero-token deterministic moves** — gloss in place, colometry, de-densification,
  organiser insertion, command-term unpack, discourse stems, render directives, spacing,
  line length, one-question-per-page — **run entirely in the browser. The document never
  leaves the device at all.**
- Only generative moves hit the server, and only with the spans they touch.
- A teacher can work on a sensitive document with the network off and still get most of the
  value.

"Your file never leaves your computer unless you ask for a rewrite" is a sentence worth more
than any compliance document, and it is now structurally true rather than a promise.

## 4. Stack

- **Next.js (App Router)**, deployed to an **EU region** — Vercel `fra1`, or Fly.io
  `fra`/`ams`, or Hetzner if cost matters. Region is recorded, and the model endpoint is a
  swappable adapter so a change of provider or region is config, not a rewrite.
- **No database in v1.** `localStorage` for the teacher's own preferences and glossaries.
  Postgres arrives with accounts, and only ever holds teacher-owned rows.
- **docx**: JSZip in, `docx` (or the same in-place XML write) out. Print via CSS
  `@media print` — no headless Chromium, ever.
- **Model**: one server route, zero-retention terms, EU inference, no training on inputs.
  Verify in the provider's data-processing terms rather than assuming.
- **No analytics that record content.** Counts only.

## 5. Adapt and generate are the same pipeline

Both are wanted. They are not two products — **generation is adaptation with an empty
source**, and that collapses the architecture.

**Adapt** (the 22:40 case, and the default). Drop in your worksheet. The task defines its
own construct; the app measures demand, proposes moves, refuses the illegal ones, exports.

**Generate** (the Sunday-planning case). Same engine, seeded by a spec instead of a document:

```
subject          Ιστορία Γ΄ Γυμνασίου | MYP4 I&S
criterion/strand D-ii  (analyse and evaluate sources)
command term     Να αξιολογήσετε  /  Evaluate
source text      [the teacher pastes the source — required]
statement of inquiry / unit  [optional, one line]
output           source-analysis task, 3 items, one lesson
```

**It fills a template; it does not invent a task.** The hard rule: **generation requires the
teacher's own source text or unit spec as a seed.** Without one it refuses, because a task
generated from nothing does not match the unit, the sequence, or what was taught last
Tuesday — and a plausible-looking task that misses the unit is worse than no task.

Everything downstream is identical: the generated task passes the same refusal engine, the
same command-term and reachability validators, the same tiering, the same two renders. A
generated task that would invalidate its own declared criterion gets refused exactly as a
teacher's would.

**Both modes declare the criterion and strand they assess, on the artefact.** That single
field is what makes the refusal engine possible in either direction.

## 6. Screens

1. **Start** — one drop zone (paste / .docx / demo). Mode toggle: **MYP** ↔ **Γυμνάσιο**.
   Subject. A second tab: **Create a task** for the generate path. Nothing else on screen.
2. **Read** — your document, with demand measured: offending spans highlighted, command
   terms detected, tier-3 terms marked, the three driving features named
   («γενική βάθος 4 · ονοματοποίηση 7,2/100 · δύο λόγιες μετοχές»). This is the moment the
   teacher decides the app understood their document. Everything depends on it.
3. **The room** — barrier chips, grouped by channel, ticked not typed. Free-text line
   accepted too («3 αργοί αναγνώστες, 2 νέοι στα ελληνικά»). Target: 15 seconds. If it takes
   longer, the ontology is too big.
4. **Plan** — proposed moves, each quoting the span of *your* text it touches, each naming
   the barrier and the strategy. Accept / reject per move. Refusals in their own panel with
   reasons, each with an override that asks you to type why.
5. **Export** — teacher copy (rationale, refusals, arrangement checklist) and student copies
   (zero diagnostic marking, zero route name a student could read as a rank). .docx download,
   print pack with correct copy counts.

Assessment mode — *teaching / formative / summative* — is a single control on screen 1 that
changes how strict the refusal engine is. On summative it fails closed.
