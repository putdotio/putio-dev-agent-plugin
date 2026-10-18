# Frontend `CONTRIBUTING.md` template

Adapt the commands, toolchain, and development notes to the repo. Keep the file focused on contributor setup and validation.

````md
# Contributing

## Setup

Install the required toolchain, then install dependencies:

```bash
<install-command>
```

## Run locally

```bash
<dev-command>
```

## Validation

Run the project checks before opening or updating a pull request:

```bash
<check-command>
<test-command>
<build-command>
```

Keep only the checks contributors are expected to run.

## Development notes

- Add only repo-specific notes that help contributors.
- Explain required environment variables, local services, or architecture constraints only when they affect day-to-day development.
- Link to deeper docs if the notes get long.

## Pull requests

- Keep changes focused and explicit.
- Add or update tests when behavior changes.
- Keep this section high-level and aligned with `.github/pull_request_template.md` when the repo has one; the template holds the full review checklist.
- Include the review evidence the pull request template asks for; upload media with `gh pr create --attach` or `gh pr comment --attach`, never commit it.
- Prefer small follow-up pull requests over mixing unrelated cleanup into feature work.
````
