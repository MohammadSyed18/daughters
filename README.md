# Daughters of Legends

## Context assembly and store integrity

The backend separates retrieval, context assembly, store auditing, ingestion, and HTTP handling into independent modules:

- [retrieval.py](DOL_Netlify_Ready_With_Legacy_AI_Coach/legacy-ai-coach/backend/retrieval.py): corpus-aware passage ranking, deduplication, and page diversity.
- [context.py](DOL_Netlify_Ready_With_Legacy_AI_Coach/legacy-ai-coach/backend/context.py): bounded reference context with stable passage identities, explicit source pages, and inspectable lexical coverage.
- [store_audit.py](DOL_Netlify_Ready_With_Legacy_AI_Coach/legacy-ai-coach/backend/store_audit.py): page/text checks, duplicate warnings, finite embedding checks, and embedding-dimension consistency.
- [app.py](DOL_Netlify_Ready_With_Legacy_AI_Coach/legacy-ai-coach/backend/app.py): validated chat requests, bounded retrieved context, source excerpts, and demo/model response paths.
- [ingest.py](DOL_Netlify_Ready_With_Legacy_AI_Coach/legacy-ai-coach/backend/ingest.py): PDF cleaning, overlapping chunks, embedding batches, integrity checks, and atomic publication.

The passages sent to the model are the same bounded selection used to populate response sources. Context excerpts are represented as JSON reference records; the system prompt treats their contents as reference material rather than instructions. Passage/source metadata is preserved without inventing page numbers. Store audits run before serving a loaded store and before publishing newly ingested chunks.

```sh
cd DOL_Netlify_Ready_With_Legacy_AI_Coach/legacy-ai-coach/backend
python -m unittest discover -s tests -v
```

Tests cover context limits, stable identities, duplicate suppression, query overlap, invalid metadata, nonfinite or inconsistent embeddings, HTTP request validation, and mocked embedding responses. Live model calls and PDF ingestion require the configured service credentials and authorized source document; the tests make no paid API requests. Lexical coverage is not a calibrated correctness probability.
