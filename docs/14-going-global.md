# Going global without generalising prematurely

The ask: keep it from being Greece-locked, in case it gets attention. The cheap version of
that is an architecture decision made now. The expensive version is building for markets
that do not exist yet. Do the first, refuse the second.

## Three pluggable packs, one engine

```
engine/                    ← pack-agnostic. Never contains a language or a curriculum.
  operations/              seven document operations
  validators/              command-term identity, reachability, coverage, novelty, egress
  refusal/                 verdict lookup, override → candidate
  docx/                    JSZip round-trip, in-place <w:p> mutation
  render/                  teacher copy, student copies, print pack

packs/
  lang/el.yaml             syllabifier rules, morphology features, readability drivers,
  lang/en.yaml             tier lists, command-term lexicon, orthography checks
  standards/myp.yaml       criteria, strands, command terms, construct verdicts, modes
  standards/gymnasio-el.yaml
  bridge/el-en.yaml        cognates, false friends, transfer errors
```

Adding **IGCSE**, **A-Level**, **DP**, **AP**, or **ACARA** is a standards pack. Adding
**Spanish** or **Arabic** is a language pack plus a bridge pack. Neither is engine work.

## The abstraction validates itself on day one, for free

The usual failure of pluggable architecture is that it is designed against one instance and
is therefore wrong. That risk does not apply here:

- You already need **two standards packs** — MYP and Γυμνάσιο — and they differ in the
  deepest way possible: one binds on construct validity, the other on statutory arrangement.
- You already need **two language packs** — el and en — and they differ in every analyser.

Two instances of each, from requirements you already have, before any speculative market.
The interfaces will be right because they were forced to be. This is luck; take it.

## What is globally valuable, and what is local strength

| | Reach |
|---|---|
| **The construct-legality verdicts — "which scaffold invalidates which assessment"** | **Global.** Every criterion-referenced system has this question. IB, IGCSE, A-Level, AP, ACARA. Nobody has answered it publicly. This is the asset. |
| The refusal engine that enforces them | Global |
| The seven document operations, the docx round-trip | Global, and commoditising |
| Bilingual command-term unpacking | Global *pattern*, local *data* — every L1 cohort has the same trap |
| The Greek analyser | Local strength, and an unusual credibility signal: "it does Greek morphosyntax properly" reads as *serious* to anyone, including people who will never use Greek |

**So the global pitch is not "a Greek differentiation tool with English support."** It is
*"the tool that tells you which scaffolds are legal for the assessment you are actually
running"* — which happens to do Greek better than anything else in existence.

## Four cheap decisions that keep the door open

1. **English-first UI, Greek available.** A strings file. A Greek-only interface caps reach
   at Greece on the first click, and it is an hour of work to avoid.
2. **No login stays.** It was the right privacy call; it is also the entire distribution
   mechanism. Anyone who sees a link can click it and watch the refusal engine work in
   twenty seconds, with a demo document pre-loaded. That is how a teacher tool spreads —
   not sales.
3. **Zero retention travels.** "We store nothing about students" is jurisdiction-independent.
   FERPA, COPPA, UK GDPR, Australian privacy principles — all become non-conversations. A
   product that stored profiles would need a legal review per market.
4. **Pack files are data, and can be community-authored.** A verdict pack for IGCSE English
   Literature can be written by an IGCSE English teacher without touching the engine. That
   is the only realistic path to coverage beyond one person's subjects, and it is also the
   answer to the bus factor.

## What not to do

- **Do not author a third standards pack** before someone asks. Two is what proves the
  interface; three is speculation with a maintenance cost.
- **Do not internationalise the UI beyond EN/EL.** Strings file, two locales, done.
- **Do not build multi-region deploy, tenancy, or accounts.** Single EU region. There is no
  student data to localise.
- **Do not rename around "global".** The Greek is a credibility asset, not an embarrassment
  to hide. Being conspicuously excellent at one hard language is the strongest available
  evidence that the rest is not hand-waving.
