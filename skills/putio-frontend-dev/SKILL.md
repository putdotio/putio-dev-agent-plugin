---
name: putio-frontend-dev
description: "put.io end-user apps and shared frontend packages: build or review UI, state, tests, docs, test harnesses and their auth, delivery, and machine setup. Use in a put.io frontend repository or for put.io frontend conventions. SDK and API client work, putio CLI consumer operations (CLI use inside a test harness stays here), browser-only put.io inspection, and non-put.io frontends belong elsewhere."
---

# put.io frontend development

The target repo's `AGENTS.md`, docs, and code precedent win over this skill.

## Start

1. Read the repo's `AGENTS.md` files (`git ls-files '*AGENTS.md'`), `README.md`,
   and the docs for the area you change.
2. Name the stack, the verify entrypoint, the delivery target, and where you
   will run the change to prove it.
3. Open the reference or [Notion page](#knowledge-base) the task needs:
   - CI, verify, release, secrets: [delivery](./references/delivery.md)
   - README, CONTRIBUTING, templates, agent docs: [docs](./references/docs.md)
   - Browser, native, TV, emulator, or device proof:
     [test harness](./references/test-harness.md)
   - Harness setup through the `putio` CLI:
     [CLI harness](./references/cli-harness.md)
   - Machine tools and repo clones:
     [environment setup](./references/environment-setup.md)

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

Edit Frontend pages; edit another team's page only when the task names it.

## Repositories

[repos.json](./repos.json) lists put.io repositories: `name` (local folder),
`repo` (GitHub remote), `path` (canonical checkout under `~/projects/putdotio/`),
`owns`, `visibility`, `topics`, and `mode`.

- Frontend repos list `frontend` in `owns`. A cross-repo task iterates those,
  includes SDKs only when the task does, and runs each repo's own verify.
- In shared repos, frontend work lives in `putdotio/.github`'s `frontend-*`
  workflows and this plugin's `putio-*` skills.
- Check `visibility` before quoting across repos: private content stays in
  private repos.

## Rules

- **Types**: parse external input at the boundary and derive types from the
  validated contract. Fix type errors there, not with casts or non-null
  assertions.
- **State**: model bug-sensitive flows as explicit states with exhaustive
  handling.
- **Errors**: show expected errors as typed, localized messages, never raw
  error text. Contain unexpected failures so unrelated UI keeps rendering.
- **Effects**: keep them in adapters and leaf components so render trees stay
  pure.
- **Credentials**: only an official put.io website takes a username, password,
  or current one-time code, and the TOTP seed stays in the authorized secret
  provider. CLI, mobile, TV, extension, and harness clients sign in through
  OAuth or device-link and handle only codes and tokens.
- **CI**: build, verify, deploy, and smoke logic lives in repo commands; CI
  calls them from thin workflow YAML.
- **Delivery**: merging or pushing to `main` is delivery, including the
  deploys it starts. Ask first for a manual deploy or publish, secret or
  provider changes, writes outside the forge, and force-pushes.
- **PR bodies**: the outcome, real risks, what stayed unverified, and one
  visual aid (screenshot, recording, Mermaid, table, or code sample). Leave out
  test counts, command logs, and review history.
- **Conventions**: record non-obvious ones in the nearest `AGENTS.md`.

## Proof

- Run the owner's checks for the changed behavior and its dependents. Run the
  full gate when the owner requires it, shared inputs changed, or coverage is
  uncertain. Report package, consumer, browser, and device proof separately.
- Exercise user-visible changes in the real browser, app, simulator, emulator,
  or device: first load, loading, error, empty, small screens, focus, and races.
  Look at the rendered result yourself; a finished run is not proof it looks right.
- Reuse passing proof while its source, inputs, and environment are unchanged.
- Name each proof you could not run and what blocked it. Finding a command
  does not authorize live actions.
- Stop the runtimes you started.

## Boundaries

- SDK repositories: the SDK development skill.
- The `putio` CLI as a consumer: the CLI's own guidance. Harnesses use it
  through the [CLI harness](./references/cli-harness.md) contract.
- Repository policy, generic Actions hardening, build-tool migrations, and
  independent code review are outside this skill.
