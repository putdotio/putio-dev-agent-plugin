# Top-level docs

Templates and section order live in Notion:
[Repo docs](https://app.notion.com/p/3fdd546ba2f581a69d8ff6c5fc6e1c79).

- `README.md` is for users (what, install, use); `CONTRIBUTING.md` is for
  contributors (setup, run, checks). Every repo has a `LICENSE`.
- Security reporting follows the
  [org-wide policy](https://github.com/putdotio/.github/blob/main/SECURITY.md);
  a repo adds `SECURITY.md` only when its policy differs.
- `docs/distribution.md` owns release and publishing; contributor docs link
  to it.
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
  write. Leave out volatile metrics such as test counts.

```bash
test -e README.md && test -e CONTRIBUTING.md && test -e LICENSE
test ! -e AGENTS.md || { test -f CLAUDE.md && ! test -L CLAUDE.md && test "$(cat CLAUDE.md)" = "@AGENTS.md"; }
```
