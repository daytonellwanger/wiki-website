# Dependencies

<!-- purpose
The external systems this product relies on — databases, queues, third-party APIs, cloud services — and how it relies on them. Notes what specific parts of each dependency are used; the product may use an external service without using every feature it offers.

This is an overview — keep each dependency brief and link to individual files in this directory for more detail. Include representative interaction patterns and links to official documentation.

This document does not record version numbers, specific endpoint URLs, API keys, or credentials.

If it describes a technology rather than a specific external system, consult [tech/](../tech/overview.md) to see if it better fits there.
-->

**agent-wiki-wiki repository.** The source content: a git repository containing markdown files organized by section. The website reads these files at build time via relative filesystem paths (e.g., `../agent-wiki-wiki` from the project root). No API calls — direct filesystem access during the build process. The source repo is read-only from the website's perspective.

**AWS S3.** Object storage for the built website. The bucket has static website hosting enabled, with `index.html` as the index document. S3's website endpoint (not the REST endpoint) is used because it serves extensionless paths like `/concepts/agentic-loop` by returning the corresponding `index.html`.

**AWS CloudFront.** CDN sitting in front of S3. The origin points to the S3 website endpoint. CloudFront caches the entire static site and serves it globally.

**AWS ACM.** Provides the TLS certificate for the custom domain. Certificates must be provisioned in `us-east-1` regardless of region because CloudFront only reads ACM certs from that region.

**AWS Route 53.** DNS service. The custom domain has an Alias (A) record pointing to the CloudFront distribution.
