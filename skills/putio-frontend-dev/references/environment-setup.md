# Environment setup

One machine layout for put.io frontend work on macOS or Linux. Install and
activate [mise](https://mise.jdx.dev) first. Repos pin Node, pnpm, and platform
toolchains in their own mise config or version files.

## Tools

```bash
mise use -g gh jq age sops github:putdotio/putio-cli
```

- `gh`: signed in with the put.io GitHub account, which also uploads PR media
- `age` and `sops`: decrypt the local secrets vault, see
  [delivery](./delivery.md#secrets)
- `putio`: the put.io CLI test harnesses use, see
  [CLI harness](./cli-harness.md)

## Repositories

Point `registry` at this skill's installed `repos.json`, then clone every
registered repository into its canonical path:

```bash
registry=<skill-dir>/repos.json
jq -r '.[][] | "\(.repo) \(.path)"' "$registry" \
  | while read -r repo checkout; do
      target="${checkout/#\~/$HOME}"
      [ -d "$target/.git" ] || gh repo clone "$repo" "$target"
    done
```

Private entries clone only for members of the GitHub organization.

## Clones and worktrees

Agent worktrees live in the coding harness's worktree folder, outside the
canonical checkouts. Run `mise trust` in every new clone or worktree that has a
mise config before running its commands.

## Check

```bash
registry=<skill-dir>/repos.json
mise doctor
for tool in gh jq age sops putio; do command -v "$tool" >/dev/null || echo "missing: $tool"; done
gh auth status
jq -r '.[][] | .path' "$registry" \
  | while read -r checkout; do [ -d "${checkout/#\~/$HOME}/.git" ] || echo "missing checkout: $checkout"; done
```

The machine is ready when the check prints no `missing` lines and
`gh auth status` shows the put.io account. The vault repository checks secrets
separately.
