# UI Components

<!-- purpose
Documents React components in components/ directory that handle layout, navigation, and content rendering.
-->

## Layout Components

**Root layout** (`app/layout.tsx`): Wraps the entire application. Sets page metadata (`title: "Agent Wiki"`, `description: "A wiki about how AI agents work"`). Establishes the three-part layout: header with GitHub link, fixed sidebar on desktop (hidden on mobile), and flexible main content area. Uses flexbox to ensure the layout fills the viewport height.

## Navigation Components

Navigation is split between server and client components to optimize for interactivity and build time.

**Server-side components** read the filesystem at build time and pass the navigation tree to client components:

- **`Sidebar`** (`components/Sidebar.tsx`): Server component visible only on desktop (`md:hidden` hidden, normal visible). Calls `getNavTree()` and passes it to `SidebarNav`. Fixed width 16rem (w-64).

- **`MobileMenu`** (`components/MobileMenu.tsx`): Server component that calls `getNavTree()` and passes it to `MobileMenuClient`. Only handles data fetching; all UI logic is in the client component.

**Client-side components** handle interactivity, styling, and active-link highlighting:

- **`SidebarNav`** (`components/SidebarNav.tsx`): Client component ('use client') that renders the navigation tree. Shows top-level items first, then sections (categories) with sorted items. Uses `usePathname()` to highlight the active link with bold text and darker color. Applies different styles for active vs. hover states.

- **`MobileMenuClient`** (`components/MobileMenuClient.tsx`): Client component that manages the drawer state. Shows a hamburger button on small screens only. When clicked, displays a full-screen drawer with a semi-transparent backdrop. Automatically closes the drawer when the user navigates to a new page (via `useEffect` on pathname change).

## Content Rendering Components

- **`MarkdownContent`** (`components/MarkdownContent.tsx`): Renders markdown HTML as an article using the `prose` class from Tailwind's typography plugin. Sets `dangerouslySetInnerHTML` with pre-rendered HTML from `markdownToHtml()`. Applies prose styling with custom code block overrides (`prose-pre:bg-transparent`, etc.).

- **`NewsletterFeed`** (`components/NewsletterFeed.tsx`): Renders a list of rendered newsletters. Shows the publication date, newsletter HTML content, and a permalink for each. Separates newsletters with horizontal rules. Includes a footer note linking to the full newsletter archive in the sidebar.

## Page Component

**Dynamic route handler** (`app/[[...slug]]/page.tsx`): Next.js catch-all route that serves either:
- Empty slug (homepage): Renders the last 5 newsletters via `NewsletterFeed`
- Non-empty slug: Loads a page via `getPage(slug)` and renders it via `MarkdownContent`

Generates static params at build time using `generateStaticParams()`, which calls `getAllSlugs()` to walk the source filesystem. Also generates metadata based on the page title.

Returns `notFound()` if the slug doesn't match any file.
