# Delivery

Workflow detail lives in the put.io Notion:

- [Delivery model](https://app.notion.com/p/3ded546ba2f58103ae8ce55b0f7a7f9d):
  targets, trusted refs, release identities
- [Release security](https://app.notion.com/p/3ded546ba2f581a39ec9e28f38e6d3fc):
  rulesets, runners, CI cost, incident runbook
- [Release workflow mechanics](https://app.notion.com/p/3fdd546ba2f58124966ce3993500204e):
  workflow YAML
- [Repo secrets setup](https://app.notion.com/p/3fdd546ba2f5813d8a36fd219d145900):
  repo secrets setup shape

Each repo's workflows and release config own its targets, triggers, and gates.

## Verify

1. Build, verify, deploy, and smoke logic lives in repo commands. CI calls one
   repo-local `verify` from thin workflow YAML; keep an existing multi-lane
   task graph's lanes.
2. End `verify` with the shared `actions/links` and `actions/scan` steps from
   [putdotio/.github](https://github.com/putdotio/.github), pinned to a tagged
   commit; its README owns the calling contract.
3. Get `verify` passing locally before touching publish or deploy automation.

## Release

- npm packages release through the shared `frontend-release-npm` workflow
  with OIDC, not a long-lived `NPM_TOKEN`.
- Secret-bearing jobs run on trusted refs, install fresh without caches, and
  pin actions to full SHAs with an exact version comment.
- Release writes use a `putio-ci` App token in the `release` Environment;
  package releases set `deployment: false`.
- Jobs that run project code never use `pull_request_target`.
- After a delivery change, run the artifact or app smoke check, or record the
  missing one as a repo gap.

## Secrets

- Default `verify` runs without secrets; secret-dependent flows get their own
  target (`live-test`, `deploy`, `release`).
- Local secrets come from a SOPS ciphertext input through a repo-owned
  `secrets-setup` that validates and writes a gitignored `0600` `.env.local`,
  plus `secrets-clean`. It fails closed, with no password-manager fallback or
  interactive login, and stays out of install hooks. `.gitignore` keeps
  `!.env.example` tracked.
- Exceptions keep their own setup: native Xcode builds, repos that
  hold signing material, and repos whose `.env.example` carries plain device or
  runtime values.
- CI reads Environment secrets or OIDC; it never decrypts the vault or calls
  `op`.
- Public docs name only the input variable, commands, and output path, never
  vault names, payload paths, recipients, or account identifiers.
