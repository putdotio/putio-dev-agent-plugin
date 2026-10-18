---
name: putio-frontend-dev
description: "Develop or review put.io end-user apps and shared frontend packages: UI, state, tests, docs, test harnesses, delivery, and frontend machine setup. Use in a put.io frontend repository or for put.io frontend conventions. Not for unrelated frontend work, browser-only put.io inspection, SDK or API client work, or putio CLI consumer operations; frontend test-harness auth setup stays in scope."
---

# put.io frontend development

Quick rules for put.io frontend work. The target repository's `AGENTS.md`,
docs, and code precedent win; details live in the put.io Notion.

## Start

1. Read the repo's `AGENTS.md` files (`git ls-files '*AGENTS.md'`), `README.md`,
   and the docs for the area you change.
2. Identify the stack, the verify entrypoint, the delivery target, and the
   runtime proof surface.
3. Open only the reference or Notion page the task needs.

## References

- CI, verify, release, secrets: [delivery](./references/delivery.md)
- README, CONTRIBUTING, templates, agent docs: [docs](./references/docs.md)
- Browser, native, TV, emulator, or device proof: [test harness](./references/test-harness.md)
- Harness setup through the `putio` CLI: [CLI harness](./references/cli-harness.md)
- Machine tools and peer clones: [environment setup](./references/environment-setup.md)

## Knowledge base

The put.io Notion [Frontend hub](https://app.notion.com/p/dd26f70efbfb4eb0ba4dce43e3e12d01),
read through the Notion MCP, owns product, spec, design, and delivery detail.
Read the matching page before deciding:

| Topic                                           | Page                                                                                    |
| ----------------------------------------------- | --------------------------------------------------------------------------------------- |
| Targets, trusted refs, release identities       | [Delivery model](https://app.notion.com/p/3ded546ba2f58103ae8ce55b0f7a7f9d)             |
| Rulesets, runners, CI cost, incident runbook    | [Release security](https://app.notion.com/p/3ded546ba2f581a39ec9e28f38e6d3fc)           |
| Where secrets live, test accounts               | [Secrets management](https://app.notion.com/p/3ded546ba2f581e19b74c6d119adb228)         |
| Workflow YAML mechanics                         | [Release workflow mechanics](https://app.notion.com/p/3fdd546ba2f58124966ce3993500204e) |
| Repo secrets setup shape                        | [Repo secrets setup](https://app.notion.com/p/3fdd546ba2f5813d8a36fd219d145900)         |
| README and CONTRIBUTING templates               | [Repo docs](https://app.notion.com/p/3fdd546ba2f581a69d8ff6c5fc6e1c79)                  |
| Schemas, state machines, errors, forms, styling | [Engineering defaults](https://app.notion.com/p/3fdd546ba2f58118ae24c5b5de65cb26)       |
| Harness layers, platform notes, examples        | [Test harness guide](https://app.notion.com/p/3fdd546ba2f581b5b682c1f78d73848b)         |

Edit only Frontend pages unless the task names another team's page.

## Repositories

- [repos.json](./repos.json) lists put.io repositories: `name` (local folder),
  `repo` (GitHub remote), `path` (canonical checkout under
  `~/projects/putdotio/`), `owns`, `visibility`, `topics`, `mode`.
- Frontend-owned entries list `frontend` in `owns`. Cross-repo tasks iterate
  those entries, skip SDKs unless the task covers them, and run each repo's own
  verify.
- In shared repos, frontend work stays in `putdotio/.github`'s `frontend-*`
  workflows and this plugin's `putio-*` skills.
- Check `visibility` before quoting across repos; private content stays out of
  public ones.

## Rules

- Parse external input at the boundary; derive types from the validated
  contract. No casts, non-null assertions, or other escape hatches.
- Model bug-sensitive flows as explicit states with exhaustive handling.
- Keep expected errors typed and localized; never render raw error text.
  Bound unexpected failures without blanking unrelated UI.
- Keep effects at adapters and leaves so render trees stay pure.
- Only an official put.io website accepts a username, password, or current
  one-time code. The TOTP seed stays in the authorized secret provider. CLI,
  mobile, TV, extension, and harness clients use OAuth or device-link flows and
  handle only codes and tokens.
- Build, verify, deploy, and smoke logic lives in repo-owned commands; CI calls
  them through thin workflow YAML.
- Merging or pushing to `main` is delivery, including the deploys it starts.
  Ask before a manual deploy or publish, secret or provider changes, writes
  outside the forge, and force-pushes.
- Pull request bodies: outcome, real risks, what stayed unverified, and one
  visual aid (screenshot, recording, Mermaid, table, or code sample). No test
  counts, command logs, or review history.
- Record non-obvious code conventions in the nearest `AGENTS.md`.

## Proof

- Run the owner's checks for the affected behavior and its dependents; run the
  full gate when owner policy requires it, shared inputs changed, or coverage
  is uncertain. Keep package, consumer, browser, and device proof separate.
- Exercise user-visible changes in the real browser, app, simulator, emulator,
  or device: first load, loading, error, empty, small screens, focus, and races.
  Inspect the result yourself.
- Reuse passing proof while its source, inputs, and environment are unchanged.
  Name unavailable proof and its blocker. Discovering a command does not
  authorize live actions.
- Stop only the runtimes you started, and confirm they are gone.

## Boundaries

- SDK repositories follow the SDK development skill.
- Operating the `putio` CLI as a consumer follows the CLI's own guidance;
  harnesses use it through the [CLI harness](./references/cli-harness.md)
  contract.
- Repository policy, generic Actions hardening, build-tool migrations, and
  independent code review are outside this skill.
