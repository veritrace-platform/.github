# Contributing to VeriTrace

These guidelines apply to every repository in the `veritrace-platform` organization unless a repository
overrides them.

## Before you start

- Read the [architecture overview](https://github.com/veritrace-platform/platform-infrastructure/blob/main/docs/architecture/overview.md)
  and the documents for the area you are changing.
- Pick a story from the [roadmap](https://github.com/veritrace-platform/platform-infrastructure/blob/main/docs/roadmap.md),
  or open an issue first.
- Set up the local environment with the
  [development setup guide](https://github.com/veritrace-platform/platform-infrastructure/blob/main/docs/guides/development-setup.md).

## Workflow

The full rules are in the
[engineering workflow](https://github.com/veritrace-platform/platform-infrastructure/blob/main/docs/guides/engineering-workflow.md).
In short:

1. Branch from `develop`: `feat/<story-id>-<slug>`, `fix/<slug>`, `docs/<slug>`, `chore/<slug>`, …
2. Commit using [Conventional Commits](https://www.conventionalcommits.org/), for example
   `feat(handover): verify pickup code attempts`.
3. Keep each pull request to one story or one coherent slice of it. Update contracts (migrations,
   OpenAPI, messaging docs, ADRs) in the same pull request.
4. Make sure `make lint test` passes and CI is green.
5. Pull requests are squash-merged into `develop`. Releases merge `develop` into `main` and are tagged
   with SemVer.

## Standards

- Everything in the repositories is in English.
- Follow the [coding standards](https://github.com/veritrace-platform/platform-infrastructure/blob/main/docs/guides/coding-standards.md).
- Never commit secrets. Use `.env` files, which are ignored, and keep `.env.example` up to date.
