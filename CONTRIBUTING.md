# Contributing to VeriTrace

These guidelines cover the backend, platform, and documentation repositories of the `veritrace-platform`
organization. The frontend repositories follow their owner's conventions.

## Before you start

- Read the [architecture overview](https://github.com/veritrace-platform/veritrace/blob/main/docs/architecture/overview.md)
  and the documents for the area you are changing.
- Pick a story from the [roadmap](https://github.com/veritrace-platform/veritrace/blob/main/docs/roadmap.md),
  or open an issue first.
- Set up the local environment with the
  [development setup guide](https://github.com/veritrace-platform/veritrace/blob/main/docs/guides/development-setup.md).

## Workflow

See the [engineering workflow](https://github.com/veritrace-platform/veritrace/blob/main/docs/guides/engineering-workflow.md).
In short:

1. Branch from `develop` and open the pull request against `develop`; `main` only receives releases.
2. Use [Conventional Commits](https://www.conventionalcommits.org/) for commit messages and pull request
   titles, for example `feat(handover): verify pickup code attempts`.
3. Change contracts (migrations, OpenAPI, messaging docs, ADRs) together with the code.
4. Merge when `make lint test` passes and CI is green, with a squash or a merge commit.

## Standards

- Everything in the repositories is in English.
- Follow the [coding standards](https://github.com/veritrace-platform/veritrace/blob/main/docs/guides/coding-standards.md).
- Never commit secrets. Use `.env` files, which are ignored, and keep `.env.example` up to date.
