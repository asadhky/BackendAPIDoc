# Knowledge Graph API — OpenAPI specification

**[→ Browse the reference](https://asadhky.github.io/BackendAPIDoc/)**

A complete OpenAPI 3.0 specification for a knowledge-graph backend, written as a
demonstration of API design and documentation practice. **There is no live server behind
it** — this is a sample, and every identifier is an `example.org` placeholder.

The service it describes stores RDF graphs in a triple store, indexes them in Solr, and
answers natural-language questions over them using Retrieval Augmented Generation.
17 operations across 13 paths, in seven areas:

| Area | What it covers |
|---|---|
| Administration | Bulk load and delete named graphs, fetch SHACL profiles |
| Configuration | Read the service's effective Solr configuration |
| Resources | Create, read and delete individual RDF resources (Turtle) |
| File Management | Upload `.ttl` files and list what has been uploaded |
| AI | RAG answers, a server-sent-event streaming variant, and summary cards |
| Solr | Pass-through proxy to the Solr API |
| SPARQL | Pass-through proxy for SPARQL queries, GET and POST |

## Viewing it

Two rendered views are published from this repository:

- **[Reference view](https://asadhky.github.io/BackendAPIDoc/)** (`index.html`) — Redoc.
  Three-panel layout, built for reading end to end.
- **[Swagger UI view](https://asadhky.github.io/BackendAPIDoc/swagger-ui.html)**
  (`swagger-ui.html`) — the familiar expand-and-explore layout. "Try it out" is disabled,
  since there is nothing to call.

To serve them locally — Redoc and Swagger UI both fetch `openapi.yaml` over HTTP and
cannot read it from `file://`:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

The spec is a plain OpenAPI 3.0 document, so Postman, Insomnia, Stoplight and OpenAPI
generators all read `openapi.yaml` directly.

## Validating

[Redocly CLI](https://redocly.com/docs/cli/) runs on every push and pull request via
`.github/workflows/lint.yml`:

```bash
npx @redocly/cli lint openapi.yaml
```

The spec validates with no errors. One warning remains — `no-server-example.com`, which
flags the `localhost` development server and is expected here.

## Design notes

A few choices worth calling out, since the point of the repo is the documentation itself:

- **Errors are first-class.** A shared `Error` schema is referenced from reusable
  `400`/`401`/`404`/`500` responses in `components.responses`, so every operation documents
  its failure modes rather than just the happy path.
- **Responses are named schemas**, not inline objects, so the spec generates usable client
  models.
- **Both auth schemes are declared and applied globally** — `BasicAuth` (HTTP Basic) and
  `SessionAuth` (a session cookie established when a session is created).
- **The streaming endpoint is documented as `text/event-stream`** with its event sequence
  spelled out, rather than being described as JSON.

## Layout

```
openapi.yaml      the specification
index.html        Redoc reference view
swagger-ui.html   Swagger UI view, submit disabled
.github/          CI workflow linting the spec
```

## Licence

MIT — see [LICENSE](LICENSE).
