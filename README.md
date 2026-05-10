# fuzzy-garbanzo

Shared GitHub Actions reusable workflows for the org.

## Workflows

### `fuzzy-garbanzo.yaml` — Docker image deployment

Builds a multi-arch (`linux/amd64`, `linux/arm64`) Docker image, pushes it to a container registry, and creates a GitHub Release. Triggered via `workflow_call`.

The image is tagged with the full semver, major.minor, and major version from the git tag.

#### Inputs

| Name | Type | Required | Description |
|---|---|---|---|
| `registry` | string | yes | Container registry host (e.g. `ghcr.io`, `docker.io`) |
| `repository` | string | yes | Image repository name (e.g. `myrepo`). Combined with `username` and `registry` to form the full image path. |

#### Secrets

| Name | Required | Description |
|---|---|---|
| `username` | yes | Registry username |
| `password` | yes | Registry password or token |

#### Usage — GitHub Container Registry

```yaml
name: Deployment
on:
  push:
    tags:
      - "v*.*.*"
jobs:
  deploy:
    uses: hcubasd/fuzzy-garbanzo/.github/workflows/fuzzy-garbanzo.yaml@v0.1.3
    with:
      registry: ghcr.io
      repository: myrepo
    secrets:
      username: ${{github.actor}}
      password: ${{secrets.GITHUB_TOKEN}}
    permissions:
      contents: write
      packages: write
```

#### Usage — Docker Hub

```yaml
name: Deployment
on:
  push:
    tags:
      - "v*.*.*"
jobs:
  deploy:
    uses: hcubasd/fuzzy-garbanzo/.github/workflows/fuzzy-garbanzo.yaml@v0.1.3
    with:
      registry: docker.io
      repository: myrepo
    secrets:
      username: ${{secrets.DOCKERHUB_USERNAME}}
      password: ${{secrets.DOCKERHUB_TOKEN}}
    permissions:
      contents: write
      packages: write
```

Permissions must be granted at the caller level as well as in the reusable workflow, since GitHub intersects the two.

## Versioning

This repo is semver tagged. Pin callers to a specific version (e.g. `@v0.1.3`) and update deliberately when a new version is cut.
