# Test harness

Layers, platform notes, and examples live in Notion:
[Test harness guide](https://app.notion.com/p/3fdd546ba2f581b5b682c1f78d73848b).

A harness wraps platform tools in typed commands, prepares auth, drives real
product flows, asserts state, and leaves proof artifacts.

- Inspect the surface, existing repo commands, docs, and auth entrypoints
  before designing anything.
- Expose repo-local, deterministic, typed commands with structured output.
  Keep platform verbs (`install`, `launch`, `screenshot`) apart from scenario
  verbs (`play-video`); product journeys and fixtures live in the app repo.
- Wire the harness into the repo's `verify`, `smoke`, or `live-test`
  entrypoint. Missing hardware or config fails early with the exact setup
  need; never skip proof silently.
- Leave screenshots, logs, state dumps, or recordings in an ignored working
  directory, and inspect them before reporting.
- Wait on observable state, not sleeps.
- Never check in device IPs, passwords, certificates, signing material,
  tokens, account identifiers, personal paths, or private content IDs.

## Auth

- Use a repository-configured test profile; never a developer's cookies,
  personal account, or ambient CLI auth. Keep profile names neutral, such as
  `<test-profile>`.
- Only an official put.io website accepts the username, password, or current
  one-time code. The TOTP seed stays in the authorized secret provider, which
  may generate the current code for process-scoped browser automation on the
  official login page.
- CLI, TV, native, extension, and harness clients use the web app's OAuth or
  device-link flow and handle only codes and tokens. No credential exchange,
  TOTP generation, or credential-login API calls in a client or harness;
  standard OAuth refresh stays allowed.
- Keep credentials and tokens out of logs, state dumps, transcripts,
  artifacts, command arguments, uploads, and checked-in files.
- Harness payloads hold only non-account test fixtures. Unattended runs use
  dedicated machine identities.
- Through the `putio` CLI, follow the [CLI harness](./cli-harness.md) contract.
