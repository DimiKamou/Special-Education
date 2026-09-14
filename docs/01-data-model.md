# Data model

> **Rewritten.** The original version of this section was SQLite DDL — `runs`, `ops`,
> `tenant_id`, `prev_hash`/`row_hash`, `reapplied_from_op_id` — plus a `classes/*.class`
> file holding student initials, dated evidence and a ΚΕΔΑΣΥ arrangement with an expiry,
> described as "the entire learner store". `docs/09` rules out any student record,
> identifier or pseudonymised handle; `docs/10` forbids storing the protocol number or the
> date; `docs/12` removes the database entirely. **Both are deleted.** That class file was
> the single document a DPO or an inclusion lead would have quoted back at you.

## What persists, and where

| | Store | Holds |
|---|---|---|
| **Session** | JS heap, dies with the tab | the barrier vector — deliberately not recoverable |
| **Device** | IndexedDB, one object store | the document, the demand analysis, declared mode/criteria, op decisions, your glossaries, your verdict candidates, URW counters keyed by *typed* `class_handle` |
| **Repo** | pack files, versioned | ontology, strategy cards, verdicts, command terms, bridge pack — all about **tasks**, never about students |

The one genuinely good rule from the deleted class file survives as a UI gate rather than a
parser rule: **the Plan screen does not unlock until at least one strength chip is set.**
A profile that is a list of deficits produces deficit output.

## The session barrier vector

```ts
// Lives in memory. No name, no initials, no identifier, no evidence dates, no arrangements
// with protocol numbers. There is no field for any of them.
type RoomProfile = Readonly<{
  class_handle: string;                       // typed by the teacher: "Γ2", "MYP4B". Not a roster key.
  barriers: ReadonlyArray<{
    code: string;
    lang: 'el' | 'en' | 'grc' | null;         // null only for codes flagged language_bound: false
    severity: 1 | 2 | 3;
  }>;
  strengths: ReadonlyArray<string>;           // >= 1, enforced by the UI gate
  arrangements_in_force: ReadonlyArray<string>;  // ticked per session: 'oral_assessment', 'extra_time'
  counts?: ReadonlyArray<{ pattern: string; n: number }>;  // "3 like this" — anonymous, for route sizing
}>;
```

`arrangements_in_force` is a tick, not a record: it constrains what the app proposes and
prints on the teacher copy. Nothing is stored about who holds it or who issued it.

## The verdict pack — the only thing that compounds

Not a table. A versioned data file, about tasks and criteria, containing zero personal data.

```yaml
# packs/verdicts/myp.is.seed.yaml
pack_version: 1
standards_pack: ib_myp_2026
rows:
  - strategy_id: S-OPVL-GRID-BLANK
    subject_group: individuals_and_societies
    strand_id: myp.is.D.iii          # IDENTIFIER ONLY. Never paraphrased IB descriptor text.
    product_type: source_response
    assessed_verb: evaluate
    verdict: legal                   # legal | conditional | invalidating
    rationale: >                     # OUR words, always
      The grid is a layout aid. The student still names the limitation, which is what
      Diii assesses.
    authored_by: DK
    authored_on: 2026-09-20
    source: seed                     # seed | override | coordinator
```

**Three defects to fix before authoring a single row** (see `docs/15-backlog.md`):

1. **No canonical strand-identifier form.** The docs write `Diii`, `D-ii`, `Ai` and `A–D`
   for the same thing. Pick `myp.is.D.iii` and use it everywhere.
2. **`strand_id` is unsatisfiable for Γυμνάσιο**, which has no criteria and no strands. The
   Γυμνάσιο pack keys on arrangement and task type instead.
3. **`assessed_verb` is in the key and no screen on the adapt path collects it.** Derive it
   from the command term the extractor already found, and confirm on ≤4 items.

**Defaults:** a missing row is `conditional` (flag and proceed) on teaching and formative;
`invalidating` (fail closed) on `summative_internal`.

**Overrides are the authoring queue.** Every refusal renders an override control; taking it
writes a `verdict_candidate` to IndexedDB with your typed reason, plus a *"download my
verdict candidates"* button. With no database, the queue is a file you own — which is better
than a table you cannot see.

**`criteria_modification` is cut from v1.** It was gated to coordinator level in three
documents and there is no login, no identity and no role model to gate on. A fourth verdict
value with an unenforceable gate is worse than its absence.

```yaml
# ontology/channels.v1.yaml — FROZEN week 1, AFTER the day-3 discrimination test.
# Rule: if no strategy card in the library selects on code A and not on code B,
# merge them. Expect 24-31 proposed codes to collapse to 12-15 real ones.
# Each survivor carries: icf_cy ref + ONE teacher-answerable observable question.
input:     [decode.rate, visual.crowding]
language:  [lex.academic_breadth, syntax.complexity, inference.load, register.distance]
load:      [wm.verbal, instruction.holding, element_interactivity]
executive: [initiation, planning.sequencing]
output:    [transcription.automaticity, encoding.spelling, discourse.organisation, production.rate]
number:    [graph.decoding, temporal.sequencing]
affect:    [evaluative.threat, predictability.need]     # text_transform_allowed: false — ENFORCED
                                                        # at card-load time, not in a prompt
```


```ts
// The model's ONLY output shape. A rewritten document is a schema rejection.
type SpanEdit =
  | { op:'insert_after'; bid:string; role:'glossary'|'stem'|'organiser'|'worked_example'
                                          |'checklist'|'command_term_card'|'extension'; node:Node }
  | { op:'annotate';     bid:string; range:[number,number]; mark:'command_term'|'tier3'|'known_lex' }
  | { op:'split';        bid:string; at:number[] }        // INDICES ONLY — code cuts, chars never retyped
  | { op:'render';       bid:string|null; directives:RenderDirectives }
  | { op:'replace';      bid:string; range:[number,number]; text:string }  // gated: explicit action, ≤12/artefact
  | { op:'drop';         bid:string; redundant_evidence_of:string[] };     // strand-union checked

// Compile-time firewall. Widening this type is the only way a name reaches a prompt.
type PromptContext = Readonly<{
  barriers: ReadonlyArray<{code:string; lang:'el'|'en'|'grc'; severity:1|2|3}>;
  demand:   ReadonlyArray<{code:string; load:0|1|2|3; bids:string[]}>;
  blocks:   ReadonlyArray<Block>;   // Block = {bid, kind, text, lang, docx_anchor, features}
  params:   Readonly<Record<string, string|number>>;
  output_language: 'el'|'en';
}>;                                 // no name, no initials, no student_ref, no label, no free text
```
