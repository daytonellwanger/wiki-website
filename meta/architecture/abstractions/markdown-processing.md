# Markdown Processing

<!-- purpose
Describes lib/markdown.ts, which converts markdown to HTML at build time.
-->

`lib/markdown.ts` exports `markdownToHtml(content)`, which processes markdown files through a remark plugin pipeline and returns HTML.

The pipeline runs the following steps in order:

1. **GitHub Flavored Markdown** — `remark-gfm` adds support for tables, strikethrough, autolinks, and other GFM extensions
2. **Link rewriting** — A custom remark plugin visits every link node and rewrites `.md` hrefs to root-relative URL paths (e.g., `concepts/memory.md` becomes `/concepts/memory`)
3. **Syntax highlighting** — A custom `remarkShiki` plugin uses shiki to colorize code blocks at build time. Supports light and dark themes (github-light, github-dark) with automatic fallback for unsupported languages
4. **HTML conversion** — `remark-html` converts the AST to HTML

The highlighter (shiki) is instantiated once and reused across builds via a promise cache, avoiding repeated initialization.

All processing happens at build time — the output is plain HTML with embedded color styles and no JavaScript.