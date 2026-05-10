# fuzzy-garbanzo

Shared GitHub Actions reusable workflows for the org.

## Workflows

### `fuzzy-garbanzo.yaml` — Docker image deployment

Builds a multi-arch (`linux/amd64`, `linux/arm64`) Docker image, pushes it to GitHub Container Registry, and creates a GitHub Release. Triggered via `workflow_call`.

The image is tagged with the full semver, major.minor, and major version from the git tag. The image name is derived from the calling repository — no inputs required.

#### Usage

```yaml
name: Deployment
on:
  push:
    tags:
      - "v*.*.*"
jobs:
  deploy:
    uses: hcubasd/fuzzy-garbanzo/.github/workflows/fuzzy-garbanzo.yaml@v0.1.0
    permissions:
      contents: write
      packages: write
```

Permissions must be granted at the caller level as well as in the reusable workflow, since GitHub intersects the two.

## Versioning

This repo is semver tagged. Pin callers to a specific version (e.g. `@v0.1.0`) and update deliberately when a new version is cut.
