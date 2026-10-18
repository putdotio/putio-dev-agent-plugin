---
name: putio-sdk-dev
description: "Develop or review put.io SDK and API client packages in TypeScript, Swift, Kotlin, and similar languages. Use in a put.io SDK repository or for put.io SDK conventions. Not for unrelated SDKs, browser-only put.io inspection, end-user application code, or putio CLI operations."
---

# put.io SDK development

Quick rules for put.io SDK work. The target repository's `AGENTS.md`, docs,
verify command, and release policy win. Scope doctrine, language notes, and
code patterns live in the put.io Notion
[SDK engineering guide](https://app.notion.com/p/3fdd546ba2f5817a9a06fc5f5f99db7b);
product direction under Frontend hub › Products.

## Start

1. Read the repo's `AGENTS.md` files and its documented verify and live-test
   commands. Never infer a Vite+, Gradle, or Make entrypoint from this skill.
2. Inspect the target namespace and the shared transport or client runtime.
3. Releasing or publishing: read [release](./references/release.md).

## Rules

- Each SDK is a public package: domain-first, strongly typed, native to its
  language (Effect-first TypeScript, `async throws` Swift, coroutine-first
  Kotlin).
- TypeScript is the canonical full API client. Swift and Kotlin stay narrower,
  widened only when backend behavior is verified and first-party apps use it,
  at the same quality bar.
- Parse at the boundary; no raw JSON results, casts, or stringly typed errors
  on the public surface. Typed errors are public API.
- When sources disagree, trust backend behavior and tests, then current
  first-party app usage, maintained SDKs, archived clients, and published docs.
  Use backend or consumer checkouts only when available and authorized.
- Preserve naming, parity, and type safety unless a documented reason
  justifies a change. Keep repo-specific guidance in that repo's `AGENTS.md`.

## Endpoint changes

1. Check backend behavior, its tests, and current app usage before widening or
   changing a contract. Find the tracked source, test, fixture, and consumer
   paths with `git ls-files | xargs rg -n "<route|field>"`.
2. Update together: request input or query model, response parser or decode
   model, operation-specific error mapping, and the public method or export.
3. Add deterministic tests for request shaping, parsing, errors, and the
   public contract.
4. Add a safe live test when behavior cannot be proven locally.
5. Keep multiple public clients aligned (Promise and Effect, for example).
6. Update README, API docs, or release notes when the public surface changes.

## Verification

- A healthy SDK repo has a deterministic verify path and a separate documented
  live-test path. A missing layer is a gap to document or fix.
- Run the canonical verify as the baseline; run dependents, installed-package,
  and consumer proof when relevant, and the full gate when mandated, shared
  inputs changed, or coverage is uncertain.
- Claim full verification only when the unit, fixture, and live layers that
  matter were exercised; deterministic tests alone are partial evidence for
  production behavior.
- Live tests against shared accounts stay non-destructive and follow the
  repo's testing rules.

## Boundaries

- Merging to `main` is delivery, including the release CI starts. Ask before
  a manual publish or release, coverage-threshold changes, and irreversible
  live writes.
- End-user application code, CLI consumer operations, browser-only
  inspection, and unrelated SDKs are outside this skill.
