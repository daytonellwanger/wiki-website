# Manual Deployment

<!-- purpose
Step-by-step instructions for manually deploying the site to production when not using the automated GitHub Actions workflow.
-->

To manually deploy the built site:

```bash
npm run build
aws s3 sync out/ s3://BUCKET --delete
aws cloudfront create-invalidation --distribution-id DIST_ID --paths "/*"
```

## What each command does

**`npm run build`** generates the static site in the `out/` directory.

**`aws s3 sync out/ s3://BUCKET --delete`** uploads all files from `out/` to S3. The `--delete` flag removes any files from the bucket that no longer exist in `out/`, ensuring the bucket contains only the current build.

**`aws cloudfront create-invalidation --distribution-id DIST_ID --paths "/*"`** clears CloudFront's cache for all paths. Without this, cached pages can persist for up to 24 hours and visitors may not see the new build immediately.

## Prerequisites

- AWS CLI installed and configured.
- Credentials with permissions to:
  - `s3:PutObject` and `s3:DeleteObject` on the target bucket.
  - `s3:ListBucket` on the target bucket.
  - `cloudfront:CreateInvalidation` on the target distribution.
