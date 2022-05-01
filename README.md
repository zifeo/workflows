# Collection of Github workflows

## Release request

Create a pull request with [Commitizen](https://commitizen-tools.github.io/commitizen/) auto changelog and version bump.

```yaml
name: Create release request

on:
  workflow_dispatch:
    inputs:
      prerelease:
        description: "Prerelease type (empty == normal)"
        required: false
        type: string

jobs:
  release-request:
    uses: zifeo/workflows/.github/workflows/release-request.yml@main
    with:
      prerelease: ${{ github.event.inputs.prerelease }}
```

| Parameter | Description | Default |
|---|---|---|
| `prerelease` | One of `alpha,beta,rc` or empty (classical release) | `` |

## Pytest and release

```yaml
name: Test and release

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  test-release:
    uses: zifeo/workflows/.github/workflows/py-test-release.yml@main
    with:
      python-matrix: '["3.7", "3.8", "3.9", "3.10"]'
      poetry-version: "1.1.12"
      python-version: "3.10"
      publish-pypi: true
    secrets:
      pypi-token: ${{ secrets.PYPI_TOKEN }}
```

| Parameter | Description | Default |
|---|---|---|
| `python-matrix` | Which Python version to use during the tests, e.g. `["3.8", "3.9", "3.10"]` | required |
| `poetry-version` | Which Poetry version to install | required |
| `poetry-install-extra-args` | Extra args for Poetry install step, e.g. `"--extras aiohttp"` | `` |
| `python-version` | Which Python version to publish the package | required |
| `docker-compose` | Whether to launch Docker companion containers during tests | `false` |
| `docker-build` | Whether to test Docker build | `false` |
| `docker-platform` | Which platform Docker to use, e.g. `linux/amd64,linux/arm64` | `linux/amd64` |
| `publish-docker` | Whether to publish in the docker registry | `false` |
| `publish-pypi` | Whether to publish to Pypi | `false` |
