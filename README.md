# Instill AI Protobufs

This repository is a fork of
[instill-ai/protobufs](https://github.com/instill-ai/protobufs/) that serves as
a playground for developing a GitHub action that syncs the public API reference,
hosted in [ReadMe](https://openapi.instill.tech/), with the [Instill
Core](https://github.com/instill-ai/core).


## Current status

There are 3 public OpenAPI references (Core, Model and VDP), which version
points to the latest Instill Core version (perhaps model should have a different
release version.

Every time a new Instill Core release is published, a [GitHub
action](https://github.com/instill-ai/core/actions/workflows/sync-version-with-api-docs.yml)
creates a PR updating the version in the OpenAPI documents. Once this is merged,
we have to manually re-sync the public documentation.

## Goals

- Having a private version of the API references that we can keep in sync with
  the `main` branch in the `protobufs` repo in order to QA the documentation
  before syncing the public version.
- Syncing the public version automatically when the version update PR is merged.

## ✅ Step 1: with a single public and private version

- On `main` branch merge: update staging docs
- On version PR merge: update production docs

### ReadMe setup

2 versions per (major) Core release: public and private.

![ReadMe versions](https://github.com/jvallesm/instill-protobufs/assets/3977183/e7c5aa6e-52db-498e-a27c-7fa1506dc089)

As we have 3 docs per version, we can either keep the IDs as variables in our
workflow or obtain them from the ReadMe API.

## ✅ Step 2: one public version per release + a staging version for the next release.

- On `main` branch merge: update staging docs
- On version PR merge: create new (main, beta, public) version with staging one
  as base.

### ReadMe setup

1 public version per Core release.
1 private version per major Core release.

![ReadMe versions](https://github.com/jvallesm/instill-protobufs/assets/3977183/e17e4a00-fbbb-4288-88ed-aafe670287ac)

We can keep the private API IDs constant, then create new main versions from
them as releases occur.

## ⏭️ Additional feature: display diff on version update PR

- On version PR merge: create tag with version
- On Core GitHub action PR: generate link with diff.
