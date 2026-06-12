# Deployment

<!-- purpose
How code gets to production. Covers all environments code passes through (e.g., development, staging, production), the process for promoting code through each, and any gates or approval steps required before a promotion.

Notes any environment-specific configuration or meaningful behavioral differences across environments.

This is an overview — keep each topic brief and link to individual files in this directory for each deployment scenario. Each scenario file should contain the concrete, step-by-step instructions a developer would follow to actually perform that deployment. If a deployment scenario exists but no file for it exists yet, create one.

Does not record specific hostnames, IP addresses, infrastructure resource IDs, or environment-specific configuration values.

If it describes how to build the code prior to deployment, consult [dev/build](../dev/build.md) to see if it better fits there. If it describes how to monitor the system after deployment, consult [observability/](../observability/overview.md).
-->

**Production.** A static Next.js export is built (`npm run build`), producing an `out/` directory of plain HTML, CSS, and JavaScript. The built site is uploaded to AWS S3 and served through CloudFront (CDN). GitHub Actions handles automated deployment on push to the main branch via OIDC authentication.
