# Setup

<!-- purpose
How to prepare a machine to contribute to this codebase. Covers installing languages, runtimes, SDKs, CLIs, and any other tooling required before the first build.

Should include both the happy path and any known platform-specific gotchas.

If it describes how to build or run the code rather than set up the environment, consult [build.md](build.md) to see if it better fits there.
-->

**Node.js and npm.** The project requires Node.js and npm. Install from https://nodejs.org/.

**Dependencies.** Install dependencies with `npm install`.

**Source repository.** The website reads markdown files from the `agent-wiki-wiki` repository at build time. The build looks for it at `../agent-wiki-wiki` relative to the project root. Clone it to that location.
