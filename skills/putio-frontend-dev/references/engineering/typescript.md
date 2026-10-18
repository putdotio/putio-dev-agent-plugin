# TypeScript repo defaults

Default TypeScript package layout: `putio-sdk-typescript`. TypeScript libraries and apps share the verify-first [delivery model](../delivery/model.md).

## Defaults

- Use Vite+ (`vp`) for install, check, build, and test flows, with local `check`, `build`, `test`, and `verify` commands.
- In CI, set up with a full-SHA-pinned `voidzero-dev/setup-vp` and run `vp install` before verification or release.
- Repos that run semantic-release in their own workflow add its CI/CD-only plugins to repo `devDependencies` only when the repo intentionally supports local release execution. Configure both the commit analyzer and release notes generator with the `conventionalcommits` preset and include `conventional-changelog-conventionalcommits` in the plugin list.
- npm release, `extra_plugins` pinning, secret-bearing job setup, and release writes follow [release security](../delivery/release-security.md#actions-and-toolchains) and its [release identity](../delivery/release-security.md#release-identity).

## Build tooling

- Use the current team default toolchain for package builds, including Vite+ and `tsdown` where appropriate.
- Keep build-tool choice behind repo scripts so the workflow model does not change when packaging details do.
