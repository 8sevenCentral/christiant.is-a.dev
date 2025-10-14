# GitHub Pages Configuration

This document explains the GitHub Pages setup for the christiant.is-a.dev domain.

## Domain Configuration

- **Custom Domain**: `christiant.is-a.dev`
- **DNS Configuration**: The domain is set up to point to GitHub Pages using A records and/or CNAME records as appropriate
- **CNAME File**: Located at the repository root, contains only `christiant.is-a.dev`

## GitHub Actions Workflow

The deployment process is automated using GitHub Actions with the following workflow:

### Workflow File
- **Location**: `.github/workflows/jekyll-deploy.yml`
- **Trigger Events**: 
  - Pushes to the `main` branch
  - Manual dispatch from the GitHub Actions UI

### Workflow Details
- **Build Environment**: Ubuntu-latest runner with Ruby 3.0
- **Jekyll Build**: Uses the repository's `Gemfile` for dependency management
- **Deployment**: Uses `actions/deploy-pages@v4` to deploy to GitHub Pages
- **Concurrency Settings**: Prevents cancellation of in-progress deployments to avoid the error that was previously occurring

### Security Permissions
- The workflow has the minimum required permissions:
  - `contents: read` - To read repository content
  - `pages: write` - To deploy to GitHub Pages
  - `id-token: write` - For authentication with GitHub Pages

## Deployment Process

1. The workflow runs automatically when changes are pushed to the `main` branch
2. The Jekyll site is built using the dependencies specified in `Gemfile`
3. The built site is deployed to GitHub Pages
4. The site is accessible at http://christiant.is-a.dev/

## Troubleshooting

### Previous Issues
There was a deployment cancellation error occurring when the system tried to cancel deployments that had already completed. This has been fixed by:

1. Setting `cancel-in-progress: false` in the workflow concurrency settings
2. Removing any manual cancellation steps that might attempt to cancel completed deployments
3. Allowing deployments to complete naturally without interruption

### Verification Steps
- Check the "Actions" tab in the GitHub repository for successful workflow runs
- Visit http://christiant.is-a.dev/ to verify the site is accessible
- Check repository Settings > Pages to confirm the custom domain is properly configured

## Files in This Repository Related to GitHub Pages

- `CNAME` - Contains the custom domain name
- `_config.yml` - Jekyll site configuration
- `.github/workflows/jekyll-deploy.yml` - GitHub Actions workflow for deployment
- `Gemfile` and `Gemfile.lock` - Dependencies for building the Jekyll site

## Requirements

- Ruby 3.0+
- Jekyll 4.x
- Bundler for dependency management