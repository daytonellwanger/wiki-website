# Abstractions

<!-- purpose
The shared abstractions and interfaces that are reused across the codebase. This is not a description of every interface — only ones used across multiple features (e.g., a "service provider" pattern, a common event type, a shared middleware interface).

For each abstraction, document its purpose, its contract, and representative examples of how it is used.

This is an overview — keep each entry brief and link to individual files in this directory for more detail.

If it describes a recurring pattern rather than a shared interface, consult [conventions/](../conventions/overview.md) to see if it better fits there.
-->

**Navigation tree.** `NavItem`, `NavSection`, and `NavTree` (from `lib/content.ts`) define the sidebar and mobile menu structure. Built at build time by scanning the source repository's directory structure and extracting page titles from markdown headings. Used by both server-side navigation components (`Sidebar`, `MobileMenu`) and client-side components (`SidebarNav`, `MobileMenuClient`).

**Server/client component pattern.** Navigation components split responsibilities: server components (`Sidebar`, `MobileMenu`) fetch the navigation tree at build time and pass it to client components (`SidebarNav`, `MobileMenuClient`) that handle interactivity and styling. This pattern avoids shipping filesystem-reading code to the browser and keeps build-time concerns separate from client-side state management.
