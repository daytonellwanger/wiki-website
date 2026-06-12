# Technology

<!-- purpose
The key technologies used in this product, how they are used, and what aspects of them are relevant. For example: if services communicate via JSON RPC, explain that pattern and link to the relevant docs.

This doesn't need to cover every dependency — only the ones that meaningfully shape how the product is built or understood. A web app doesn't need to explain TCP.

This is an overview — keep each technology's description brief and link to individual files in this directory for more detail.

This document does not describe dependencies of the system. It should not include products or packages. "AWS S3" doesn't belong in this document, but "JSON RPC" does.

If it describes a specific external system or API the product relies on, consult [dependencies/](../dependencies/overview.md) to see if it better fits there. If it describes how the system is decomposed into components, consult [architecture/](../architecture/overview.md).
-->

**Static generation.** Next.js with `output: 'export'` produces a folder of pre-rendered HTML files at build time. No server is needed for production.

**Markdown processing.** The source content is markdown files stored in a separate repository. A remark plugin pipeline processes them at build time: GitHub Flavored Markdown, syntax highlighting with shiki, link rewriting, and conversion to HTML.

**Styling.** Tailwind CSS with the typography plugin for prose styling. Responsive layout: fixed sidebar on desktop, hamburger menu on mobile. Global styles in `app/globals.css` configure shiki syntax highlighting with dual theme support (light by default, dark via `prefers-color-scheme`), and prose element styling.
