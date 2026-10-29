# Top-level docs

Section order and templates for README, CONTRIBUTING, and PR and issue
templates live in Notion:
[Frontend repo docs](https://app.notion.com/p/3fdd546ba2f581a69d8ff6c5fc6e1c79).

- `README.md` is for users (what, install, use); `CONTRIBUTING.md` is for
  contributors (setup, run, checks). Every repo has a `LICENSE`.
- Security reporting follows the
  [org-wide policy](https://github.com/putdotio/.github/blob/main/SECURITY.md);
  a repo adds `SECURITY.md` only when its policy differs.
- Code, tests, workflows, and release config are the doc. No
  `docs/distribution.md`, behaviour specs, architecture overviews, testing
  guides, or harness and lane catalogs; a rule a test pins is documented by
  that test. A step a human must take or a hazard no check catches (a store
  submission, a version pin, a setting outside the repo) gets a line in
  `AGENTS.md` or `CONTRIBUTING.md` that links its owner. Extra docs are for
  users of a shipped package or for license text.
- Docs use kebab-case names, such as `docs/migrating-v12.md`.
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
