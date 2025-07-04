<!-- markdownlint-disable-next-line first-line-h1 -->
[![OpenSSFScorecard](https://api.securityscorecards.dev/projects/github.com/nginx/aws-marketplace-publish/badge)](https://scorecard.dev/viewer/?uri=github.com/nginx/aws-marketplace-publish)
[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)

# Publish Docker images to AWS Marketplace

This is a simple GitHub Action to publish new versions of Docker images to AWS Marketplace.
At the moment, it only supports adding a new version of an existing product. Contributions are welcome!

## Usage

```yaml
name: Publish to AWS Marketplace

on:
  push:
    tags:
      - 'v[0-9]+.[0-9]+.[0-9]+'

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4.2.1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Publish to AWS Marketplace
        uses: nginx/aws-marketplace-publish@v1.0.8
        with:
          product-id: ${{ secrets.AWS_MARKETPLACE_PRODUCT_ID }}
          version: ${{ replace(github.ref, 'refs/tags/', '') }}
          registry: 709825985650.dkr.ecr.us-east-1.amazonaws.com/your-repo:tag
          release-notes: |
            - New feature
            - Bug fixes
          description: |
            This is a description of your product.
          usage-instructions: |
            This is how you use your product.
```

## Inputs

| Name | Description | Required |
| --- | --- | --- |
| `product-id` | The ID of the product to publish. | Yes |
| `version` | The version of the product to publish. | Yes |
| `registry` | The Docker image to publish. | Yes |
| `release-notes` | Release notes for the new version. | Yes |
| `description` | Description of the product. | Yes |
| `usage-instructions` | Usage instructions for the product. | Yes |
