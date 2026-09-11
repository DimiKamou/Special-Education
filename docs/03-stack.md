# Stack

**One process, one laptop, one user.** Python 3.12 + FastAPI serves the API and the built SPA; `uv` for dependencies; `just run` on `localhost:7331`.

- **Storage:** SQLite (stdlib, WAL) + raw `.docx` blobs in `~/.diodos/docs/` + text files (`classes/`, `glossaries/`, `strategies/`, `ontology/`, `verdicts/`, `preferences.md`) in a folder he puts in a private git repo. Numbered `.sql` migrations; `0001` carries `tenant_id` on every table.
- **Documents:** `python-docx` for both parse and in-place apply — not pandoc, not `docx` (dolanmiu), not a rebuild. The write path mutates the object he uploaded, rebuilding run sequences from a captured formatting template.
- **Greek:** own syllabifier; own regex/suffix analysers for the triad; `spylls` (pure-Python hunspell) with the LibreOffice `el_GR` dictionary; `wordfreq` for a coarse frequency band. No native build steps, no model downloads.
- **English:** `textstat`, reported as a band with a stated confidence caveat.
- **Model:** `pip install -U "anthropic[aws]"`; `AnthropicAWS()` with `AWS_REGION=eu-central-1`, behind a three-method adapter so `AnthropicBedrock(aws_region="eu-central-1")` — AWS EMEA SARL as the named EU sub-processor — is a constructor swap the day a DPO asks for that specific paper. `Anthropic()` against `api.anthropic.com` is never used: no EU inference geo. Strict structured outputs (`output_config.format`, `additionalProperties:false`); note assistant prefill is removed on Opus 5 / Sonnet 5 and structured outputs are incompatible with the citations feature — ground with `bid` references instead.
- **Frontend:** Vite + React 19 + TypeScript, ~1,200 lines, no component library, no state manager. Server-rendered diff HTML; React owns the change cards, the chip board and the keyboard map. Accessibility as a build constraint from day one — never colour as the sole channel on barrier families, real focus order, visible focus ring, `aria-live` on op decisions — with **no conformance claim** until someone audits it.
- **Observability:** JSONL to disk plus the SQLite `runs` table. No Langfuse, no OTel, no Sentry — he is the only user and `tail -f` is the dashboard.
- **Eval:** `uv run eval` over the golden set, in a pre-commit hook.

**Running cost:** ~€0.20–0.45 per worksheet for three routes with caching and additive-default (cheaper than the rewrite-heavy estimate, because additive ops are short). Under €12/month at six worksheets a week — less than the ChatGPT subscription it replaces for this task.
