# Stack

> Rewritten. The previous version specified Python 3.12 + FastAPI, SQLite with `tenant_id`,
> `python-docx`, `spylls`, `textstat`, `wordfreq` and a server-rendered diff. All of it was
> superseded by `docs/09` (zero retention) and `docs/12` (browser runtime). Do not restore it.

**A static site plus at most one server route.** No database, no accounts, no tenancy —
there is no student data to store and no second tenant to isolate.

## Runtime

- **Next.js (App Router) + TypeScript + React.** Deployed to a single **EU region**
  (Vercel `fra1`, Fly.io `fra`/`ams`, or Hetzner). No component library, no state manager.
- **`engine/` is pure TypeScript with zero Node APIs**, so the same code runs in the browser
  and in tests. This is the load-bearing constraint: it is what makes the deterministic path
  client-side, and therefore what makes *"your file never leaves your computer"* true.

## Documents

- **JSZip + the browser's `DOMParser`/`XMLSerializer`.** Open the package, locate each
  `<w:p>`, mutate in place, re-zip. Never a rebuild, never `pandoc`, never `docx`
  (dolanmiu) for the write path.
- Mechanics that bite, all of them discovered by the audit and all of them cheap now and
  expensive later:
  - `getElementsByTagNameNS(W_NS, 't')` — never `getElementsByTagName('w:t')`.
  - Strip the BOM before `DOMParser` and check for a `<parsererror>` element; Chrome returns
    one instead of throwing.
  - Re-prepend the XML declaration verbatim, CRLF included.
  - Build new nodes by `cloneNode(true)` from an existing node in the same document, never
    `createElementNS`, or every insert carries a redundant `xmlns:w`.
  - `xml:space="preserve"` on any `w:t` with a leading or trailing space — it is what holds
    the blanks in a fill-in-the-blank exercise.
  - Strip `w:numPr` from cloned paragraphs, or inserting an organiser after Q3 renumbers Q4
    to Q5 across the whole sheet, silently, on paper, in a real room.
- **Fallback if canonical round-trip fails** (see `docs/15-backlog.md` tier 0): do not
  re-serialise `document.xml` at all — locate each `<w:p>` by byte range in the raw string
  and splice replacement XML as text. Makes the untouched-blocks assertion trivially true
  instead of hopefully true.
- **Memory ceiling is real.** JSZip holds the zip plus inflated XML in heap; a 20 MB .docx
  is ~200 MB and dies on a tablet. Declare last-two-versions of Chrome/Edge/Firefox/Safari,
  no tablets in v1, feature-detect and show a banner rather than a stack trace.
  `showSaveFilePicker` is Chromium-only — download via a blob anchor.

## Greek and English analysers

All of it ports to TypeScript. The four Python dependencies resolve as:

| Was | Now |
|---|---|
| own syllabifier | ports unchanged — it was always hand-written |
| `spylls` + `el_GR` (EL-01 accentuation) | **server-side on generated spans only** — EL-01 only ever inspects text the model produced, and that text has by definition already left the device, so this adds zero egress. The minimal-pair guard (πότε/ποτέ, πώς/πως, ή/η, τι/τί) stays client-side as a ~2 KB table |
| `textstat` | a banded word/sentence-length heuristic. No band number is displayed for Greek anyway (`docs/00`) |
| `wordfreq` | a shipped top-50k frequency list |
| `spaCy el_core_news_md` | **cut from v1.** Subordination depth gets a surface heuristic |
| Morpheus / CLTK | cut from v1. `AG-01` stays — it is a substring check and needs no data |

## Storage

- **IndexedDB, one object store.** Not `localStorage`: 5 MB, synchronous, string-only, and a
  real worksheet with a logo is 2–20 MB.
- Holds: the document, the demand analysis, the declared mode/criteria, op decisions, the
  teacher's own glossaries and verdict candidates, and URW counters keyed by typed
  `class_handle`.
- **Deliberately does not hold the barrier ticks.** On recovery the app says *"re-tick the
  room"* — 15 seconds, and the thing deliberately lost is visible to the user, which makes
  the zero-retention claim stronger rather than weaker.

## Model

- One server route, EU region, zero-retention provider terms, no training on inputs.
  Swappable adapter with the region recorded.
- Strict structured outputs, `additionalProperties: false`. `strategy_id` is set by code,
  echoed by the model, verified on return — which is simultaneously the anti-name-dropping
  mechanism *and* a genuine structural defence against prompt injection from the teacher's
  own document.
- Four cache breakpoints and an integration test asserting `usage.cache_read_input_tokens > 0`.
  That failure is silent, permanent, and 3× on the bill.
- **Whether v1 has this route at all is an open decision** — see `docs/15-backlog.md`. The
  standing default is no server: deterministic-only, with the generative path unlocked by a
  BYO API key.

## Observability

Not "none" — that was true for one user on a laptop and false the moment it is a public URL.

- Server-side: `{stage, error_class, byte_size, docx_feature_flags, browser}`. **No content.**
- Client: a "this document failed" button submitting a structural fingerprint (part names,
  element counts, which refused features are present) behind an explicit preview of the exact
  JSON being sent. That fingerprint corpus is the highest-value dataset this project can
  collect and it contains nothing about anyone.

## Test layer

- **Property test** guarding the premise: `apply(ops, doc)` contains every original character
  of every untouched block, exactly, in order, over generated adversarial documents.
- A **positive and a negative fixture per blocking validator** — a blocking validator with no
  negative fixture silently stops firing.
- A committed **horrible-docx corpus**: nested tables, `w:txbxContent`, pre-existing tracked
  changes, `w:proofErr` noise, Pages export, Google Docs export, Word 2010, polytonic,
  `xml:space` runs. Plus one LibreOffice-opens-the-export check — the colleague opening your
  file is as likely to be on LibreOffice as Word, and it is far less forgiving.
- Split the eval: deterministic checks in pre-commit; generative eval as an explicit command
  plus a nightly, with the cost printed per run. A pre-commit hook that costs €3 and four
  minutes gets `--no-verify`'d in week two.

## Cost

The €0.20–0.45 per worksheet figure was a single-laptop number and does not survive a public
URL. Before launch: a provider-level hard budget action that disables the key, an app-level
daily counter that disables **only** generative moves while leaving the client-side path fully
working, and an edge token bucket. This product degrades gracefully by construction; say so on
screen.
