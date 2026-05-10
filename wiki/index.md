# LLM Wiki

A curated, AI-distilled knowledge base over the source material in [`raw/`](../raw/).

## How this is organized

- [`raw/`](../raw/) — original sources (PDFs, transcripts, notes). **Never edited.**
- [`wiki/concepts/`](concepts/) — distilled concept pages. Each one carries YAML front-matter with `id`, `title`, `sources`, `related`, and `tags`. A `manual: true` flag protects hand-written pages from being overwritten by the ingest pipeline.
- `wiki/graph.json` — node/edge data, regenerated from the `related:` field on each concept page.

## Concepts

### Foundations
- [Tokenization](concepts/tokenization.md)

### Architecture
*Stubs — pages will be added as material is ingested.*
- Residual stream
- nanoGPT architecture

### Training
*Stubs.*
- Scaling laws (Chinchilla)
- RLHF pipeline

## Adding to the wiki

Once the ingest CLI ships (Phase 1):

1. Drop a new source into `raw/<topic>/`.
2. Run `wiki ingest raw/<topic>/<file>`.
3. Review proposed pages; merge or hand-edit. Set `manual: true` in front-matter on any page you don't want overwritten.
4. Run `wiki graph` to refresh `graph.json`.

Until then, concept pages are hand-written.
