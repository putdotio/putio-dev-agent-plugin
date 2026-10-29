# Test harness

Layers, platform notes, and examples live in Notion:
[Test harness guide](https://app.notion.com/p/3fdd546ba2f581b5b682c1f78d73848b).

- Extend the repo's existing harness commands and auth entrypoints before
  adding new ones.
- Expose repo-local, deterministic, typed commands with structured output.
  Platform verbs (`install`, `launch`, `screenshot`) stay apart from scenario
  verbs (`play-video`); product journeys and fixtures live in the app repo.
- Wire the harness into the repo's `verify`, `smoke`, or `live-test`
  entrypoint. Missing hardware or config fails early and names the exact setup
  it needs.
- Write screenshots, logs, state dumps, or recordings to an ignored working
  directory.
- Wait on observable state, not sleeps.
- Checked-in files carry no device IPs, passwords, certificates, signing
  material, tokens, account identifiers, personal paths, or private content IDs.

## Auth

- Use a repository-configured test profile, never a developer's cookies,
  personal account, or ambient CLI auth. Public examples say `<test-profile>`.
- Browser automation on the official login page may take the current one-time
  code from the secret provider for that process only.
- Harnesses never exchange credentials, generate TOTP, or call a
  credential-login API; standard OAuth refresh is fine.
- Keep credentials and tokens out of logs, state dumps, transcripts,
  artifacts, command arguments, and uploads.
- Harness payloads hold only non-account test fixtures. Unattended runs use
  dedicated machine identities.
- Through the `putio` CLI, follow the [CLI harness](./cli-harness.md) contract.
