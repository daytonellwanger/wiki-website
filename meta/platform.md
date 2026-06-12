# Platform

<!-- purpose
What kind of thing is this? An iOS app, a VS Code extension, a cloud service, a CLI tool?

This document names the platform and notes any platform-specific constraints or conventions that affect development.

This is a very brief document and does not describe architecture, does not record specific values like version numbers, package identifiers, SDK targets, project IDs, or URLs.

If it describes a technology pattern, consult [tech/](tech/overview.md) to see if it better fits there. If it describes how the system is decomposed into components, consult [architecture/](architecture/overview.md).
-->

A static website. Built with Next.js and deployed as a folder of pre-generated HTML files with no server runtime. Runs in the browser only — all rendering and markdown processing happens at build time.
