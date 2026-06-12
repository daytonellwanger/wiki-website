# Architecture

<!-- purpose
How the system is decomposed into major components and how those components relate to each other. Which things are services, which are libraries, which are jobs. What are the boundaries, what crosses them, and how.

If the system uses a common architecture pattern (e.g., microservices, client/server, kernel, event-driven), briefly describe it and link to relevant docs. Deviations from the normal pattern should be noted.

This is an overview — keep each component's description brief and link to individual documents in this directory for more detail.

This document does not include configuration values, environment-specific details, specific hostnames or ports, or implementation details within individual components.

If it describes a specific technology, consult [tech/](../tech/overview.md) to see if it better fits there. If it describes how data moves through the system, consult [data-flow/](data-flow/overview.md).
-->

A build-time static site generator with four layers:

**Build entrypoint** (`app/` directory): Next.js app routes. The root layout wraps the sidebar and content area side-by-side. The dynamic `[[...slug]]/page.tsx` route loads and renders a page for each markdown file. At build time, `generateStaticParams` walks the source filesystem and produces one static HTML file per `.md` file.

**Content layer** (`lib/content.ts`): Filesystem helpers that walk the source repository, extract page titles, and build the navigation tree. Separates file-system concerns from rendering logic.

**Markdown pipeline** (`lib/markdown.ts`): Converts markdown to HTML using remark plugins. Applies syntax highlighting (shiki), rewrites relative `.md` links to root-relative URLs, and handles GitHub Flavored Markdown.

**UI components** (`components/` directory): React components for layout, navigation, and content rendering. `Sidebar` and `MobileMenu` are server components that read the filesystem and build the nav tree; they pass it to client components (`SidebarNav`, `MobileMenuClient`) that handle interactivity and active-link highlighting. `MarkdownContent` and `NewsletterFeed` render pre-generated HTML. See [ui-components.md](abstractions/ui-components.md) for details.
