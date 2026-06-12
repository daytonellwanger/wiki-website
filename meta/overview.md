# Overview

<!-- purpose
The highest-level view of what this system does and why it exists. What problem does it solve, and for whom?

This document may also state explicit non-goals — what the system is _not_ trying to do. For a large product, the broad goal can be described here with links to `docs/` for individual features.

This document is closely related to the user-facing product documentation.

This is an overview — keep each topic brief and link to individual sections of this documentation for more detail.

This document does not inventory features, include specific metrics (user counts, revenue, performance numbers), or describe technical implementation — those belong in other documents.

If it describes a specific feature rather than the product as a whole, consult [docs/](docs/) to see if it better fits there. If it describes business or strategic context, consult [business/](business/overview.md).
-->

A statically-generated website that renders the agent-wiki markdown collection as a navigable, multi-page wiki. Each `.md` file becomes its own page. Inter-document links work seamlessly. A persistent left sidebar lists all pages organized by section.

The website is generated at build time — no server is required. The output is a folder of HTML files that can be hosted on any static host.
