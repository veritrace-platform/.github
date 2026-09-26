# Contributing to VeriTrace

These guidelines apply to every repository in the `veritrace-platform` organization unless a repository
overrides them.

## Before you start

- Read the [architecture overview](https://github.com/veritrace-platform/veritrace/blob/main/docs/architecture/overview.md)
  and the documents for the area you are changing.
- Pick a story from the [roadmap](https://github.com/veritrace-platform/veritrace/blob/main/docs/roadmap.md),
  or open an issue first.
- Set up the local environment with the
  [development setup guide](https://github.com/veritrace-platform/veritrace/blob/main/docs/guides/development-setup.md).

## Workflow

The full rules are in the
[engineering workflow](https://github.com/veritrace-platform/veritrace/blob/main/docs/guides/engineering-workflow.md).
In short:

1. Branch from `develop`: `feat/<slug>`, `fix/<slug>`, `docs/<slug>`, `chore/<slug>`, …
2. Commit using [Conventional Commits](https://www.conventionalcommits.org/), for example
   `feat(handover): verify pickup code attempts`.
3. Open the pull request **against `develop`**. `main` is the default branch and only receives releases.
   The title follows the commit format; the description is optional.
4. Keep each pull request to one coherent change, and update contracts (migrations, OpenAPI, messaging
   docs, ADRs) in the same pull request.
5. Make sure `make lint test` passes and CI is green.
6. Work pull requests are squash-merged into `develop`. Release pull requests merge `develop` into `main`
   with a merge commit (never squash).

## Standards

- Everything in the repositories is in English.
- Follow the [coding standards](https://github.com/veritrace-platform/veritrace/blob/main/docs/guides/coding-standards.md).
- Never commit secrets. Use `.env` files, which are ignored, and keep `.env.example` up to date.
