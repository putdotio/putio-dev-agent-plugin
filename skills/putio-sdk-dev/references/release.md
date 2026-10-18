# SDK release supply chain

For publishing packages, signing artifacts, creating releases, or building
distributable binaries. A stricter target-repo policy wins.

put.io SDKs follow the frontend release policy in the put.io Notion, under
Frontend / Delivery:

- [Delivery model](https://app.notion.com/p/3ded546ba2f58103ae8ce55b0f7a7f9d):
  trusted refs and release identities
- [Release security](https://app.notion.com/p/3ded546ba2f581a39ec9e28f38e6d3fc):
  rulesets, runners, and the supply-chain incident runbook
- [Secrets management](https://app.notion.com/p/3ded546ba2f581e19b74c6d119adb228):
  CI credentials

Check live provider settings before claiming a security posture.

## Mechanics

- **Refs**: release from a verified `main` commit, a protected `v*` tag, or a
  manual ref validated in a secretless job.
- **Credentials**: load narrowly scoped publish or signing credentials from a
  protected GitHub Environment or OIDC, only at that step. Prefer registry
  trusted publishing; for npm, Trusted Publishing with `id-token: write` and
  provenance.
- **Triggers**: workflows that run repo code, install, build, test, package,
  sign, or publish never use `pull_request_target`.
- **Pins**: pin release, publish, upload, and signing actions to full commit
  SHAs with an exact version comment so Renovate can update them.
- **Installs**: credentialed jobs install fresh with the pinned toolchain and
  frozen lockfile, sharing no dependency or generated-tree cache with pull
  requests. Verify downloaded toolchains and archives before using them.
- **Order**: run the repo's full verification, inspect the packed package or
  built artifact, then load credentials and publish.
- **Versions**: build versioned releases from the release tag, with manifest
  version, tag, package metadata, and public repo URL aligned.
- **Handoff**: the registry package or GitHub Release asset is the durable
  artifact, not an Actions artifact. Verify the published artifact matches
  what you inspected, then promote it without rebuilding, and record the commit SHA, tag, package version, artifact
  digest, workflow run, and published artifact identity.
- **Docs**: release and publishing behavior lives in the repo's
  `docs/distribution.md`.
