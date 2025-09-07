# simple-oidc-example-package

A minimal example package demonstrating npm Trusted Publishing with OIDC (OpenID Connect).

## Overview

This package serves as a reference implementation for setting up npm Trusted Publishing, enabling secure package publishing without storing npm tokens in CI/CD environments.

## Features

- **Token-less Publishing**: Publishes to npm using OIDC authentication instead of long-lived tokens
- **GitHub Actions Integration**: Automated release workflow with GitHub Actions
- **Provenance Attestation**: Automatically generates cryptographic attestations for package builds
- **Secure Release Process**: Two-step release process with PR review before publishing

## Workflows

### 1. Create Release PR (`create-release-pr.yml`)

Creates a release pull request with version bump and auto-generated release notes.

**Trigger**: Manual workflow dispatch with version type selection (patch/minor/major)

**Steps**:
1. Bumps package version
2. Generates release notes from previous release
3. Creates a draft PR with release changes

### 2. Release (`release.yml`)

Automatically publishes to npm when a release PR is merged.

**Trigger**: 
- When PR with `Type: Release` label is merged
- Manual workflow dispatch with specific version

**Steps**:
1. Checks if release tag already exists (idempotency)
2. Publishes package to npm with provenance
3. Creates GitHub Release
4. Comments on PR with release status

## Setup Instructions

### Prerequisites

- npm account with 2FA enabled
- GitHub repository
- Node.js LTS version

### Configure npm Trusted Publisher

1. Go to your package settings on [npmjs.com](https://www.npmjs.com/)
2. Navigate to the "Trusted Publisher" section
3. Add GitHub Actions configuration:
   - **Organization/User**: Your GitHub username or org
   - **Repository**: Repository name
   - **Workflow filename**: `release.yml`
   - **Environment** (optional): Leave empty

### Repository Setup

1. **Enable GitHub Actions PR creation**:
   - Go to Settings → Actions → General
   - Enable "Allow GitHub Actions to create and approve pull requests"

2. **Add release label**:
   - Create a label named `Type: Release` in your repository

3. **Add workflows**:
   - Copy the workflow files from `.github/workflows/` to your repository

## Usage

### Creating a Release

1. Go to Actions → Create Release PR
2. Select version type (patch/minor/major)
3. Review and edit the generated PR
4. Merge the PR to trigger automatic publishing

### Manual Release

You can also trigger a release manually:
1. Go to Actions → Release
2. Enter the version to publish
3. Run the workflow

## Security Best Practices

- **No npm tokens in CI**: Uses temporary OIDC tokens instead
- **Provenance generation**: Cryptographically verifiable build attestations
- **Token publishing disabled**: After setup, disable token-based publishing in npm settings
- **Required 2FA**: Enforces two-factor authentication for all maintainers

## Requirements

- npm CLI version 11.5.1 or higher (for OIDC support)
- GitHub-hosted runners (self-hosted runners not currently supported)
- Public repository (private repos cannot generate provenance)

## Links

- [npm Trusted Publishing Documentation](https://docs.npmjs.com/trusted-publishers)
- [GitHub OIDC Documentation](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Package on npm](https://www.npmjs.com/package/simple-oidc-example-package)

## License

MIT