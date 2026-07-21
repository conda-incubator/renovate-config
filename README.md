# Renovate config

Renovate defaults for Python and conda projects, and the organization default
for `conda-incubator`.

## Organization default

Renovate recognizes the repository and file name
`conda-incubator/renovate-config/default.json` as the default configuration for
the `conda-incubator` organization. When Renovate onboards a new repository in
the organization, it automatically proposes this preset:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>conda-incubator/renovate-config"]
}
```

The exact `default.json` name is required for automatic organization-level
onboarding.

## Public preset

Projects outside the organization can use the same configuration:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>conda-incubator/renovate-config"]
}
```

The preset builds on `config:recommended`, enables pre-commit updates, pins
GitHub Actions to immutable digests, and recognizes annotations such as these.

**GitHub Actions workflow**

```yaml
env:
  # renovate: datasource=conda depName=conda-forge/conda-build
  CONDA_BUILD_VERSION: '26.5.0'

steps:
  - ...
  - run: conda install "conda-build==$CONDA_BUILD_VERSION"
  - ...
```

**Dockerfile**

```dockerfile
# renovate: datasource=conda depName=conda-forge/conda-build
ARG CONDA_BUILD_VERSION=26.5.0
RUN conda install "conda-build==$CONDA_BUILD_VERSION"
```

**Makefile**

```makefile
# renovate: datasource=conda depName=conda-forge/conda-index
CONDA_INDEX_VERSION := 0.12.1

install:
	conda install "conda-index==$(CONDA_INDEX_VERSION)"
```

**Conda environment**

```yaml
name: your-project
channels:
  - conda-forge
dependencies:
  # renovate: datasource=conda depName=conda-forge/pytest
  - pytest=8.4.1
  - pip
  - pip:
      # renovate: datasource=pypi
      - uvicorn==0.35.0
```

Repositories can add their own Renovate settings alongside `extends` to
override or augment this preset.
