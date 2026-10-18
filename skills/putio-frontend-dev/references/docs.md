# Top-level docs

Templates and section order live in Notion:
[Repo docs](https://app.notion.com/p/3fdd546ba2f581a69d8ff6c5fc6e1c79).

- `README.md` is for users (what, install, use); `CONTRIBUTING.md` for
  contributors (setup, run, checks); every repo has a `LICENSE`.
- Security reporting follows the
  [org-wide policy](https://github.com/putdotio/.github/blob/main/SECURITY.md);
  add a repo `SECURITY.md` only when its policy differs.
- Release and publishing details live in `docs/distribution.md`; contributor
  docs link to it.
- Conventional root files keep their names (`README.md`, `CONTRIBUTING.md`,
  `LICENSE`, `SECURITY.md`, `AGENTS.md`, `CLAUDE.md`); every other doc is
  kebab-case, such as `docs/distribution.md`.
- PR and issue templates hold the review checklist; `CONTRIBUTING.md` only
  summarizes. Templates name the `gh pr create --attach` media route.
- When a repo uses `AGENTS.md`, `CLAUDE.md` is a regular file containing only
  `@AGENTS.md`, not a copy or a symlink: some plugin installers and fetchers
  reject repositories that contain symlinks.
- Use repo-relative links; verify every command, path, link, and badge you
  write. No volatile metrics such as test counts.

```bash
test -e README.md && test -e CONTRIBUTING.md && test -e LICENSE
test ! -e AGENTS.md || { test -f CLAUDE.md && ! test -L CLAUDE.md && test "$(cat CLAUDE.md)" = "@AGENTS.md"; }
```
