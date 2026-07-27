# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Developers exploring an unfamiliar codebase (their own, a dependency, or one they just joined) inside an AI coding assistant workflow. They run `/graphify` on a project, then open the generated `graph.html` in a browser to build a mental model of the codebase's structure and relationships before diving into files.

## Product Purpose

graphify maps a folder of code, docs, PDFs, images, and video into a queryable knowledge graph, so a developer can query relationships and trace paths instead of grepping through files. Output is three files: `graph.html` (interactive force-directed graph), `GRAPH_REPORT.md` (key concepts, surprising connections, suggested questions), and `graph.json` (the full graph, queryable without re-reading source files).

## Positioning

Code is parsed with tree-sitter AST into a real, traversable graph: deterministic, no LLM, nothing leaves the machine. This is not a vector index or embeddings-based similarity search — it is a graph you can traverse, query, and explain edge by edge. (Docs, PDFs, images, and video use the assistant's model or a configured API key for a semantic pass; code parsing itself stays local and deterministic.)

## Operating Context

Primary design surface for this init: `graph.html`, produced by `graphify/exporters/html.py` (`export.py` also emits an Obsidian vault, `graph.json`, and `graph.svg`; `callflow_html.py` produces a separate Mermaid architecture/call-flow HTML view). Users install via `uv tool install graphifyy` or `pipx`, register the skill with their AI assistant (`graphify install`), then invoke `/graphify .` from within Claude Code, Cursor, Codex, Gemini CLI, GitHub Copilot, and other supported assistants. The generated `graph.html` is opened directly in a browser — click nodes, filter, search.

Pipeline: `detect() → extract() → build_graph() → cluster() → analyze() → report() → export()`.

## Capabilities and Constraints

- Every edge is labeled `EXTRACTED` (explicit in source, e.g. an import or direct call), `INFERRED` (a reasonable deduction, e.g. call-graph second pass or co-occurrence), or `AMBIGUOUS` (uncertain, flagged for human review in `GRAPH_REPORT.md`).
- Nodes are grouped into detected communities (clustering), shown as colors/legend in `graph.html`.
- Supports many source languages via tree-sitter (Python, JS/TS, Go, Rust, Java, C/C++, Ruby, C#, Kotlin, Scala, PHP, Swift, and more).
- Deployment/runtime constraints for `graph.html` were not confirmed in this session (e.g. whether it must remain a single offline static file with no external network calls) — treat as undecided rather than assumed.

## Evidence on Hand

- `docs/graph-hero.png` — screenshot of `graph.html` showing the FastAPI codebase as a force-directed graph with a community legend.
- `docs/demo-path.svg` — terminal demo of a path query (`graphify explain "APIRouter"`, shortest-path lookup between concepts).
- `README.md` documents real CLI output examples (e.g. `graphify explain` output with Source/Community/Degree fields).
- No customer testimonials, benchmarks beyond `BENCHMARKS.md`, or pricing/licensing claims should be fabricated; `BENCHMARKS.md` and `docs/how-it-works.md` exist as real supporting docs.

## Product Principles

- Local and deterministic first: code structure comes from AST parsing, not an LLM guess, so the graph is trustworthy and reproducible.
- Provenance over black-box inference: every relationship is traceable back to EXTRACTED vs. INFERRED vs. AMBIGUOUS, never presented as uniformly certain.
- Traverse, don't just retrieve: the product is a real graph to explore (paths, neighbors, communities), not a similarity search over embeddings.
- Fit the existing workflow: output lands as plain files (`graph.html`, `.md`, `.json`) usable inside the AI assistant the developer already works in, with no new backend to run.

## Accessibility & Inclusion

Not established in this session — no product-specific accessibility requirement was confirmed. Record as undecided rather than assumed.
