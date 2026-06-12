# Build

<!-- purpose
How to compile or bundle the code. Covers the commands needed to produce a runnable or shippable artifact, and any flags or environment variables that affect the build.

If it describes setting up the environment before building, consult [setup.md](setup.md) to see if it better fits there. If it describes deploying the built artifact, consult [deployment/](../deployment/overview.md).
-->

**Development server.** Run `npm run dev` to start a local dev server with hot reload (uses Turbopack for fast rebuilds).

**Production build.** Run `npm run build` to produce a static export. The output is a folder of HTML files in `out/`.

**Preview.** Run `npm start` to preview the static build locally on a dev server.

The build walks the source markdown repository, processes each file through the remark pipeline (GitHub Flavored Markdown, syntax highlighting, link rewriting), generates one HTML page per markdown file, and produces a static site in `out/`. No server runtime is needed for production.
