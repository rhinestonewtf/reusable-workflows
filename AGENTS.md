# reusable-workflows

Shared GitHub Actions tooling for Rhinestone repos: reusable workflows (`.github/workflows/*.yaml`, invoked with `workflow_call`) and composite actions (`.github/actions/*/action.yaml`) that consumer repos pull in via `uses: rhinestonewtf/reusable-workflows/...@main`. Not a runtime service — see [README § Architecture](README.md#architecture) for the full picture: who calls this, what it calls, and why the lack of version pinning matters.

## Before touching a workflow or action file

- **There is no way to test a change here in isolation.** This repo has no build/test suite of its own (the only CI here is `lint-actions-pinning.yaml`, which just checks SHA-pinning). The only real test is running a consumer's CI against your branch (`uses: rhinestonewtf/reusable-workflows/.github/workflows/<file>@<your-branch>`) before merging.
- **Nearly every known consumer pins `@main`.** Merging to `main` changes CI behavior for every consumer repo's next run, immediately, with no version bump and no opt-out. Read a change here as "deploying to all consumers," not as editing a docs/config file. See the consumer table and pinning note in [README § What calls this](README.md#what-calls-this).
- **The consumer list is a lower bound, not a census.** It's built from `gh search code` plus a local-checkout grep; private repos not indexed by code search (confirmed for `eco-arbiter`, `relay-arbiter`, `relay-arbiter-v2`) won't show up unless checked out locally. Don't assume a workflow is unused just because you can't find a caller.
- **`actions/checkout` inside these workflows checks out the calling repo, not this one.** Any `run:` step (`forge build`, `pnpm test`, etc.) executes against the consumer's code. `forge-release.yaml` and `forge-artifacts.yaml` additionally assume the consumer repo provides `./build-artifacts.sh` / `./shell/prepare-artifacts.sh` respectively — neither script lives in this repo. `release.sh` at this repo's root is unreferenced by any workflow here.
- **Third-party actions must be SHA-pinned; `rhinestonewtf/*` actions are exempt** (`.pinact.yaml`), by convention pinned to `@main` instead. `lint-actions-pinning.yaml` enforces the third-party half on every PR touching workflow/action files.
- **No AWS, no cloud credentials, no ECR.** The only credential this repo mints is a short-lived GitHub App installation token (`setup-gh-access`, via `actions/create-github-app-token`), used solely to let consumer builds `pnpm install`/`forge build` against private Rhinestone dependencies.

## Docs

- [README](README.md) — workflow/action catalog, usage examples, and the Architecture section (system diagram, consumers, what it calls, datastore/deploy/intent-lifecycle answers, blast radius)
