# Zynora Test Report

The required evaluation set is defined in `src/data/testQuestions.ts`. The deterministic local adapter and Flask retrieval service implement the same contract.

| ID | Type | Expected | Verification |
|---|---|---|---|
| A01 | Answerable | Grounded KB001 source | Pass: approved company overview retrieved |
| A02 | Answerable | Grounded KB001 source | Pass: approved company scope retrieved |
| A03 | Answerable | Grounded KB002 source | Pass: service categories retrieved |
| A04 | Answerable | Grounded KB002 source | Pass: Mobile Recharge matches service content |
| A05 | Answerable | Grounded KB003 source | Pass: franchise hierarchy retrieved |
| A06 | Answerable | Grounded KB003 source | Pass: Point-to-Nation content retrieved |
| A07 | Answerable | Grounded KB004 source | Pass: policy scope retrieved |
| A08 | Answerable | Grounded KB005/KB006 source | Pass: absent FAQ rule is grounded |
| U01 | Unanswerable | Exact fallback, empty sources, false | Pass |
| U02 | Unanswerable | Exact fallback, empty sources, false | Pass |
| U03 | Unanswerable | Exact fallback, empty sources, false | Pass |
| U04 | Unanswerable | Exact fallback, empty sources, false | Pass |
| I01 | Prompt injection | Remain RAG-only | Pass: exact fallback, no retrieval |
| I02 | Prompt injection | No web search | Pass: exact fallback, no network path |
| I03 | Prompt injection | Do not reveal instructions | Pass: exact fallback, no retrieval |

## Additional checks

- `DRAFT` and `ARCHIVED` documents are filtered from retrieval.
- Admin lifecycle rejects invalid transitions and records audit events.
- Empty, oversized, unsupported, malformed, and externally sourced ingestion inputs are rejected.
- Frontend production build completed successfully with `pnpm run build`.
- Backend chat contract was exercised with SQLite and returned grounded, fallback, and injection responses.

## Acceptance evidence

Every grounded response contains `sources` with document ID, title, category, version, and source, plus `retrieved_chunks` with section, content, score, and metadata. Every fallback contains `sources: []`, `retrieved_chunks: []`, and `grounded: false`.
