# Content Loading

<!-- purpose
Describes lib/content.ts, which handles filesystem access and content metadata extraction.
-->

`lib/content.ts` provides functions to read markdown files from the source repository and build navigation data structures.

**`getAllSlugs()`** walks the source filesystem recursively and returns slug arrays for every `.md` file (except README.md). Used by Next.js `generateStaticParams` to determine which pages to pre-render at build time.

**`getPage(slug)`** reads a markdown file by its slug and returns its content and extracted title (parsed from the first `# Heading` in the file). Returns `null` if the file doesn't exist.

**`getNavTree()`** builds the complete navigation structure by:
1. Reading `index.md` to extract section order from level-2 headings
2. Listing top-level files (`index.md`, `overview.md`, `log.md`) in display order
3. Walking subdirectories, sorting them by the order specified in `index.md`, and extracting titles from each file's first heading

The nav tree is split into `topLevel` (root files) and `sections` (subdirectories with their contents). Used by server-side layout components to render the sidebar.

**`getRecentNewsletters(n)`** returns the n most recent newsletter files from a `newsletters/` subdirectory, formatted with human-readable dates.

All functions work at build time only — no runtime filesystem access.