# Top-level docs

Section order and templates for README, CONTRIBUTING, and PR and issue
templates live in Notion:
[Frontend repo docs](https://app.notion.com/p/3fdd546ba2f581a69d8ff6c5fc6e1c79).

- `README.md` is for users (what, install, use); `CONTRIBUTING.md` is for
  contributors (setup, run, checks). Every repo has a `LICENSE`.
- Security reporting follows the
  [org-wide policy](https://github.com/putdotio/.github/blob/main/SECURITY.md);
  a repo adds `SECURITY.md` only when its policy differs.
- `docs/distribution.md` owns release and publishing; contributor docs link
  to it. Name the durable handoff (tested output, Release asset, registry
  package, image digest, provider package), never Actions artifacts.
- Docs use kebab-case names, such as `docs/distribution.md`. Conventional root
  files (`README.md`, `CONTRIBUTING.md`, `LICENSE`, `SECURITY.md`, `AGENTS.md`,
  `CLAUDE.md`) and platform-required names (`.github/pull_request_template.md`,
  `.github/ISSUE_TEMPLATE/`, `CODEOWNERS`, `SKILL.md`) keep theirs.
- PR and issue templates hold the review checklist; `CONTRIBUTING.md`
  summarizes it. Templates name the media route: `gh pr create --attach` or
  `gh pr comment --attach` (gh 2.99+).
- With `AGENTS.md`, `CLAUDE.md` is a regular file containing only `@AGENTS.md`.
  Some plugin installers reject repositories with symlinks.
- Use repo-relative links, and check every command, path, link, and badge you
  write. Leave out volatile metrics such as test or coverage counts.
- Mask user PII, and call third-party apps "ecosystem apps".

```bash
test -e README.md && test -e CONTRIBUTING.md && test -e LICENSE
test ! -e AGENTS.md || { test -f CLAUDE.md && ! test -L CLAUDE.md && test "$(cat CLAUDE.md)" = "@AGENTS.md"; }
```
