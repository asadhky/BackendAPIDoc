# Knowledge Graph API — OpenAPI specification

A **sanitized** OpenAPI 3.0 specification for a backend knowledge-graph service, plus a
ready-to-serve Swagger UI page for browsing it.

The service it describes stores RDF graphs in a triple store, indexes them in Solr, and
answers natural-language questions over them using Retrieval Augmented Generation. The API
covers seven areas: graph administration, configuration, individual RDF resources, file
uploads, RAG answer and card endpoints, a Solr proxy, and a SPARQL query proxy.

All hostnames, identifiers and payloads have been replaced with `example.org` placeholders.
No internal servers, credentials or data are exposed.

## Browsing the docs

The repository ships a Swagger UI page (`index.html`) that loads `openapi.yaml` directly.

**Hosted:** enable GitHub Pages for this repository (Settings → Pages → deploy from `main`,
root) and the reference is served at `https://asadhky.github.io/BackendAPIDoc/`.

**Locally**, any static file server works — Swagger UI cannot read `openapi.yaml` over
`file://`:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

**Or use your own tooling** — the spec is a plain OpenAPI 3.0 document, so
[Redoc](https://github.com/Redocly/redoc), Postman, Insomnia and OpenAPI generators all
read it as-is.

## Validating

The spec is checked by [Redocly CLI](https://redocly.com/docs/cli/) on every push via
`.github/workflows/lint.yml`. To run the same check locally:

```bash
npx @redocly/cli lint openapi.yaml
```

Two warnings are expected and intentional: `info-license` (see below) and
`no-server-example.com`, which flags the `localhost` development server.

## Authentication

Two schemes are declared, and the spec applies both globally — every operation requires one
of them:

- `BasicAuth` — HTTP Basic.
- `SessionAuth` — a session identifier established when a session is created.

## Layout

```
openapi.yaml   the specification: 17 operations across 13 paths
index.html     Swagger UI, loading ./openapi.yaml
.github/       CI workflow that lints the spec
```

## Licence

No licence is attached. This spec documents a service the author did not solely author, so
reuse terms are not the author's alone to grant — treat it as read-only reference material
unless you have been told otherwise.
