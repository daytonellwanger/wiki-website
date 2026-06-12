# Data Flow

<!-- purpose
How data moves through the system. Where does it enter, where does it exit, and what happens in between?

This is an overview — keep each flow brief and link to more detailed pages in this directory for specific paths or subsystems.

If it describes what a core entity means rather than how it moves, consult [data-model/](../data-model/overview.md) to see if it better fits there.
-->

**Page rendering:** At build time, `generateStaticParams()` walks the source repository via `getAllSlugs()` to discover all pages. For each page, `getPage()` reads the markdown file and `markdownToHtml()` converts it to HTML. The dynamic route handler (`page.tsx`) serves either the rendered page or the recent newsletter feed. All HTML is pre-generated, and no data flows at runtime.

**Navigation metadata:** At build time, `getNavTree()` scans the source repository to extract page titles and build the sidebar structure. This metadata is passed from server components (`Sidebar`, `MobileMenu`) to client components (`SidebarNav`, `MobileMenuClient`) at build time and serialized into the static HTML. The client uses `usePathname()` to determine the active link without further data fetching.
