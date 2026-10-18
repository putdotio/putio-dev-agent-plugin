# Delivery

Workflow detail lives in Notion:
[Release workflow mechanics](https://app.notion.com/p/3fdd546ba2f58124966ce3993500204e),
[Repo secrets setup](https://app.notion.com/p/3fdd546ba2f5813d8a36fd219d145900),
and the Delivery pages linked from SKILL.md. Each repo's `docs/distribution.md`
owns its targets, triggers, and gates.

## Verify

- Inspect package scripts, Makefile, workflows, docs, and `.env.example` before
  editing delivery.
- One repo-local `verify` entrypoint that CI calls; preserve an existing
  multi-lane task graph instead of collapsing it.
- `verify` ends with the shared `actions/links` and `actions/scan` steps from
  [putdotio/.github](https://github.com/putdotio/.github), pinned to a tagged
  commit. Its README owns the calling contract.
- Make `verify` pass locally before touching publish or deploy automation.
- TypeScript repos use Vite+ (`vp`) with `check`, `build`, `test`, and `verify`
  commands.

## Release

- npm packages release through the shared `frontend-release-npm` workflow with
  OIDC; no long-lived `NPM_TOKEN`.
- Secret-bearing jobs run on trusted refs, install fresh without caches, and
  pin actions to full SHAs with an exact version comment.
- Release writes use a `putio-ci` App token in the `release` Environment;
  package releases set `deployment: false`.
- No `pull_request_target` for jobs that run project code.
- After a delivery change, run the artifact or app smoke check, or record the
  missing one as a repo gap.
- Update `docs/distribution.md` in the same change when release, cache,
  provenance, or signing behavior changes.

## Secrets

- Default verify runs without secrets; secret-dependent flows get their own
  target (`live-test`, `deploy`, `release`).
- Local secrets: a SOPS ciphertext input, a repo-owned `secrets-setup` that
  validates and writes a gitignored `0600` `.env.local`, and `secrets-clean`.
  Fail closed; no password-manager fallback or interactive login.
- Keep `secrets-setup` out of install hooks. `.gitignore` keeps
  `!.env.example` tracked.
- CI never decrypts the vault or calls `op`; it reads Environment secrets or
  OIDC.
- Public docs name only the input variable, commands, and output path, never
  vault names, payload paths, recipients, or account identifiers.
