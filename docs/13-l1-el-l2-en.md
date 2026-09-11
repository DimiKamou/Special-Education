# The actual cohort: L1 Greek, L2 English

Correction to an earlier assumption. The cohort is **not** Greek-as-a-second-language
learners. It is Greek native speakers studying through English. That inverts the bridge
direction and produces the single best feature in the product.

## Why this cohort is not "EAL" in the usual sense

Generic EAL tooling is built for learners with no lexical overlap with English. A Greek L1
student is the opposite case: they hold an enormous latent English academic vocabulary they
do not know they hold, and a small set of confident errors nobody ever corrects.

**The barrier is not tier-3. It is tier-2.** The technical vocabulary of MYP humanities is
largely Greco-Latinate and therefore transparent to a Greek speaker — `analysis / ανάλυση`,
`synthesis / σύνθεση`, `hypothesis / υπόθεση`, `criterion / κριτήριο`,
`phenomenon / φαινόμενο`, `hierarchy / ιεραρχία`, `chronology / χρονολογία`,
`democracy / δημοκρατία`, `crisis / κρίση`, `metaphor / μεταφορά`, `irony / ειρωνεία`.

What actually stops them is the connective, hedging and stance vocabulary of academic
English — `however`, `nevertheless`, `whereas`, `albeit`, `undermine`, `account for`,
`arise from`, `in the light of`, `to a large extent`, `thereby`, `insofar as`. Coxhead
Academic Word List territory. Invisible to a teacher, because the student clearly
understands the *content*.

## Feature 1: the cognate bridge

Show the Greek root alongside the English academic term, in the margin, at the moment it
appears. Not a translation — a **bridge**: *you already own this word*.

No existing tool does this, because no existing tool assumes a specific L1. It is only
possible because the cohort is homogeneous, which is exactly the thing that makes a niche
tool better than a general one.

**And the false friends, which matter more.** These are confident, invisible, repeated
errors that cost marks and never get diagnosed:

| English | Student reads it as | Actually means | Greek for the real sense |
|---|---|---|---|
| empathy | εμπάθεια *(malice, animosity)* | understanding another's feelings | ενσυναίσθηση |
| sympathetic | συμπαθητικός *(likeable, charming)* | showing compassion / in agreement with | συμπονετικός |
| apology | απολογία *(a defence, a plea)* | an expression of regret | συγγνώμη |
| pathetic | παθητικός *(passive)* | pitiful | αξιολύπητος |
| idiot | ιδιώτης *(private citizen)* | fool | ηλίθιος |
| sycophant | συκοφάντης *(slanderer)* | flatterer | κόλακας |

A Greek student writing "the poet is sympathetic" in a Literature response means something
different from what the examiner reads. That is a Criterion A mark lost to a lexical
accident, and it is trivially fixable — once.

The false-friend list is a **bridge pack**: a data file, not code. `el↔en` first. Any other
L1 is another file.

## Feature 2: the command-term trap

A Greek L1 student reading *"To what extent was the Treaty responsible for…"* frequently
parses it as a yes/no question. They then answer it as one, competently, and receive a low
**Criterion D** mark.

That mark says "weak critical thinking." The truth is a comprehension failure in the
question stem. The student's analytical ability was never measured.

This is the most common invisible mark-loss in a bilingual MYP room, and the fix is one
line in the margin:

> **To what extent** — *σε ποιον βαθμό.* Not yes or no. You must weigh how much, and say
> what pulls the other way.

**Therefore: bilingual command-term unpacking is a blocking default, not an option**, on
every English-language task in a Greek L1 room. It is the highest-yield single feature for
this cohort and it costs zero model tokens — it is a lookup table of ~60 MYP command terms
with a Greek gloss and a "what the examiner wants" line.

## What this changes

- **Both analysers are load-bearing.** Greek, for Λογοτεχνία / Νεοελληνική / Ιστορία in
  Greek. English, for MYP taught in English. Not one primary and one afterthought.
- **The bridge is a third pack type**, alongside language packs and standards packs.
- **Bilingual is the default render, not a setting.** Greek marginal support on an English
  document is the normal case in this room, not an accommodation.
- **Dyslexia in a bilingual student presents differently per language** and the profile must
  stay split. Greek is shallow for reading and deep for spelling; English is deep for both.
  A student can be fluent-but-misspelling in Greek and slow-but-accurate in English, and
  both are the same underlying deficit. `el:decode.rate=3 · en:decode.rate=1` is not a
  contradiction — it is the expected pattern.
