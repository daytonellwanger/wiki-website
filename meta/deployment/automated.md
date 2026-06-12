# Automated Deployment

<!-- purpose
The GitHub Actions workflow that automatically deploys the site to production. Describes how the workflow is triggered, what it does, how it authenticates to AWS, and what prerequisites are required to set it up.
-->

The deployment workflow is defined in [`.github/workflows/deploy.yml`](../../.github/workflows/deploy.yml).

## Triggers

- **On push to main:** Automatically deploys any commit pushed to the main branch.
- **Manual trigger:** Can be manually triggered via GitHub's workflow dispatch interface.

## Workflow steps

1. **Checkout code:** Clones the website repository.
2. **Clone content repo:** Clones the `agent-wiki-wiki` repository containing source markdown files (placed in `../agent-wiki-wiki` relative to the project root).
3. **Setup Node.js:** Installs Node 20 with npm cache enabled.
4. **Install dependencies:** Runs `npm ci` to install exact dependency versions.
5. **Build:** Runs `npm run build` to generate the static site in the `out/` directory.
6. **Configure AWS credentials:** Authenticates to AWS using OIDC, assuming an IAM role (role ARN stored in `AWS_ROLE_ARN` secret).
7. **Sync to S3:** Runs `aws s3 sync out/ s3://BUCKET --delete` to upload built files and remove deleted files.
8. **Invalidate CloudFront:** Runs `aws cloudfront create-invalidation` to clear the CDN cache, ensuring visitors see the new build immediately.

## Prerequisites

### GitHub secrets

- `AWS_ROLE_ARN`: The ARN of the IAM role to assume (e.g., `arn:aws:iam::ACCOUNT:role/ROLE_NAME`).
- `S3_BUCKET`: The S3 bucket name (used in the sync step).
- `CLOUDFRONT_DISTRIBUTION_ID`: The CloudFront distribution ID (used in the invalidation step).

### AWS setup

The workflow uses OIDC to authenticate without long-lived credentials. This requires:

1. **OIDC provider:** An Identity Provider in AWS IAM pointing to `token.actions.githubusercontent.com`.
2. **IAM role:** A role with a trust policy allowing GitHub Actions to assume it (scoped to this repository).
3. **Permission policy:** The role must have permissions to:
   - `s3:PutObject` and `s3:DeleteObject` on the target bucket.
   - `s3:ListBucket` on the target bucket.
   - `cloudfront:CreateInvalidation` on the target distribution.

See `reference/hosting.md` in the source repository for example policies.
