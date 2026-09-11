# Data model

```sql
-- SQLite v1 (WAL). tenant_id present from migration 0001 on EVERY table so the
-- month-6 Postgres+FORCE RLS port is mechanical. No table anywhere holds a name,
-- a date of birth, or a diagnosis.

-- 0001_init.sql ------------------------------------------------------------
CREATE TABLE runs (
  run_id TEXT PRIMARY KEY, tenant_id TEXT NOT NULL DEFAULT 'local',
  doc_sha256 TEXT NOT NULL, class_file_sha256 TEXT,
  mode TEXT NOT NULL,          -- teaching | formative | summative_internal | summative_external
  curriculum TEXT NOT NULL,    -- ib_myp | gr_national
  subject_group TEXT, declared_criteria TEXT,        -- JSON: ['B','D'] — 3 clicks, never inferred
  item_strand_tags TEXT,       -- JSON, REQUIRED only when mode='summative_internal'
  ontology_version TEXT, library_version TEXT, analyser_version TEXT,
  prompt_bundle_version TEXT, verdict_pack_version TEXT,
  model_id TEXT, model_region TEXT,                  -- 'eu-central-1'
  plan_json TEXT, refused_json TEXT, validation_json TEXT,
  tokens_in INT, tokens_cached INT, tokens_out INT, cost_eur REAL, latency_ms INT,
  exported_at TEXT, printed INT DEFAULT 0,
  created_at TEXT NOT NULL, prev_hash TEXT NOT NULL, row_hash TEXT NOT NULL
);

CREATE TABLE ops (
  op_id TEXT PRIMARY KEY, run_id TEXT NOT NULL, tenant_id TEXT DEFAULT 'local',
  strategy_id TEXT NOT NULL,          -- set by CODE, echoed by model, verified on return
  bid TEXT NOT NULL,                  -- stable ULID of the block it touched
  kind TEXT NOT NULL,                 -- additive | rewrite | render
  barrier_codes TEXT NOT NULL,        -- JSON
  demand_codes TEXT NOT NULL,
  quoted_span TEXT NOT NULL,          -- verbatim slice of HIS text — renders on the change card
  payload TEXT NOT NULL,              -- the SpanEdit
  access_delta INT NOT NULL, demand_delta INT NOT NULL DEFAULT 0,
  fade_class TEXT NOT NULL,           -- atl_taught | permanent_arrangement | universal_design
  provisional INT NOT NULL DEFAULT 0, -- 1 = barrier was unevidenced; see permission gate
  decision TEXT, edited_text TEXT, decided_at TEXT,
  -- the outcome signal that is actually capturable:
  reapplied_from_op_id TEXT,          -- set when the same card+student-set recurs on a later doc
  edit_distance_on_reapply REAL
);

CREATE TABLE verdicts (               -- SCHOOL-LOCAL POLICY, not global IP. Seeded ~15 rows.
  tenant_id TEXT DEFAULT 'local',
  strategy_id TEXT NOT NULL,
  standards_pack TEXT NOT NULL,       -- 'ib_myp_2026'
  subject_group TEXT NOT NULL,
  strand_id TEXT NOT NULL,            -- IDENTIFIER ONLY. Never paraphrased IB descriptor text.
  product_type TEXT NOT NULL,         -- essay | source_response | oral | data_response | *
  assessed_verb TEXT NOT NULL,        -- evaluate | analyse | justify | * (from the clarification)
  verdict TEXT NOT NULL CHECK (verdict IN ('legal','conditional','invalidating','criteria_modification')),
  rationale TEXT NOT NULL,            -- OUR words, never IB's
  authored_by TEXT, authored_on TEXT, source TEXT,  -- 'seed' | 'override' | 'coordinator'
  pack_version INT NOT NULL,
  PRIMARY KEY (tenant_id, strategy_id, standards_pack, subject_group,
               strand_id, product_type, assessed_verb, pack_version)
);
-- DEFAULTS: missing row → 'conditional' (flag + proceed) on teaching/formative;
--           missing row → 'invalidating' (fail closed) on summative_internal.
CREATE TABLE verdict_candidates (     -- THE AUTHORING QUEUE. Every refusal override lands here.
  id TEXT PRIMARY KEY, tenant_id TEXT, run_id TEXT, strategy_id TEXT,
  strand_id TEXT, product_type TEXT, assessed_verb TEXT,
  teacher_reason TEXT NOT NULL, created_at TEXT, promoted_to_verdict INT DEFAULT 0
);
```

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

```
# classes/9B-istoria.class  — the entire learner store. A text file in a private git repo.
class: 9B Ιστορία | programme: MYP4 | loi: en | roll: 24

ΝΚ   el:decode.rate=2 [obs 2026-09-03]
     el:encoding.spelling=3 [report 2025-11-12]
     wm.verbal=2                                   # no [evidence] → PROVISIONAL, still generates
     + oral.fluency, visual.reasoning              # >=1 strength or the line fails to parse
     arr: KEDASY:oral_exam:2027-06-30
     arr: SCHOOL:laptop:2027-06-30
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
