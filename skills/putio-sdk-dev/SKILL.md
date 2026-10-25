---
name: putio-sdk-dev
description: "put.io SDK and API client packages in TypeScript, Swift, Kotlin, and similar languages: build, review, or release them. Use in a put.io SDK repository or for put.io SDK conventions. End-user app code, putio CLI operations, browser-only put.io inspection, and non-put.io SDKs belong elsewhere."
---

# put.io SDK development

The target repo's `AGENTS.md`, docs, verify command, and release policy win
over this skill. Scope doctrine, language notes, and code patterns live in the
put.io Notion
[SDK engineering guide](https://app.notion.com/p/3fdd546ba2f5817a9a06fc5f5f99db7b);
product direction under Frontend hub › Products.

## Start

1. Read the repo's `AGENTS.md` files and use the verify and live-test commands
   its docs name.
2. Read the target namespace and the shared transport or client runtime.
3. For a release or publish, read [release](./references/release.md).

## Rules

- **Shape**: each SDK is a public package, domain-first, strongly typed, and
  native to its language: Effect-first TypeScript, `async throws` Swift,
  coroutine-first Kotlin.
- **Scope**: TypeScript is the canonical full API client. Swift and Kotlin
  stay narrower and widen only for verified backend behavior that first-party
  apps use, at the same quality bar.
- **Types**: parse at the boundary. The public surface exposes parsed types
  and typed errors, which are public API.
- **Sources of truth**, in order: backend behavior and tests, current
  first-party app usage, maintained SDKs, archived clients, published docs.
  Read backend or consumer checkouts when available and authorized.
- **Stability**: keep naming, parity, and type safety unless a documented
  reason says otherwise. Repo-specific guidance goes in that repo's
  `AGENTS.md`.

## Endpoint changes

1. Check backend behavior, its tests, and current app usage before widening or
   changing a contract.
2. Update together: request input or query model, response parser or decode
   model, operation-specific error mapping, and the public method or export.
3. Add deterministic tests for request shaping, parsing, errors, and the
   public contract.
4. Add a safe live test when local tests can't prove the behavior.
5. Keep parallel public clients aligned, such as Promise and Effect.
6. Update README, API docs, or release notes when the public surface changes.

## Verification

- A healthy SDK repo has a deterministic verify path and a separate,
  documented live-test path. Fix or document a missing layer as a gap.
- Run the canonical verify as the baseline. Add dependents, installed-package,
  and consumer proof when relevant, and the full gate when the repo requires
  it, shared inputs changed, or coverage is uncertain.
- Deterministic tests are partial evidence for production behavior. Claim
  full verification only after exercising the unit, fixture, and live layers
  that matter.
- Live tests against shared accounts stay non-destructive and follow the
  repo's testing rules.

## Delivery

- Merging to `main` is delivery, including the release CI starts. Ask first
  for a manual publish or release, coverage-threshold changes, and
  irreversible live writes.
