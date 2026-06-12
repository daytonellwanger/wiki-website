# GitHub Actions AWS Credentials Setup

<!-- purpose
How to set up the AWS authentication for the GitHub Actions deployment workflow. Covers the OIDC provider setup, IAM role creation, trust policy, and permission policy.
-->

The [automated deployment workflow](automated.md) authenticates to AWS using OIDC (OpenID Connect) rather than long-lived access keys. At runtime, GitHub exchanges a short-lived OIDC token for temporary AWS credentials.

## One-time AWS setup

### Identity Provider

Create an Identity Provider in AWS IAM:
- Provider type: OpenID Connect
- Provider URL: `https://token.actions.githubusercontent.com`
- Audience: `sts.amazonaws.com`

### IAM Role

Create an IAM role for GitHub Actions with the following trust policy:

```json
{
  "Effect": "Allow",
  "Principal": {
    "Federated": "arn:aws:iam::YOUR-ACCOUNT-ID:oidc-provider/token.actions.githubusercontent.com"
  },
  "Action": "sts:AssumeRoleWithWebIdentity",
  "Condition": {
    "StringEquals": {
      "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
    },
    "StringLike": {
      "token.actions.githubusercontent.com:sub": "repo:YOUR-ORG/agent-wiki-website:*"
    }
  }
}
```

This trust policy scopes the role to the specific GitHub repository, allowing any branch or workflow in that repo to assume the role.

### Permission Policy

Attach a permission policy to the role with the minimum required permissions:

```json
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:PutObject", "s3:DeleteObject"],
      "Resource": "arn:aws:s3:::YOUR-BUCKET/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::YOUR-BUCKET"
    },
    {
      "Effect": "Allow",
      "Action": "cloudfront:CreateInvalidation",
      "Resource": "arn:aws:cloudfront::YOUR-ACCOUNT-ID:distribution/YOUR-DIST-ID"
    }
  ]
}
```

Replace `YOUR-BUCKET` and `YOUR-DIST-ID` with the actual S3 bucket name and CloudFront distribution ID.

## GitHub Repository Setup

In the GitHub repository, add these secrets:

- `AWS_ROLE_ARN`: The ARN of the IAM role created above (e.g., `arn:aws:iam::ACCOUNT:role/ROLE_NAME`).
- `S3_BUCKET`: The S3 bucket name where the site is deployed.
- `CLOUDFRONT_DISTRIBUTION_ID`: The CloudFront distribution ID used to serve the site.

The [workflow file](../../.github/workflows/deploy.yml) references these secrets when authenticating and deploying.
