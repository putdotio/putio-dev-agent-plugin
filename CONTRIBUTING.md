# Contributing

## Setup

Install [pnpm](https://pnpm.io/installation), then:

```bash
pnpm install --frozen-lockfile
```

## Change a skill

Skill layout, frontmatter, picker metadata, and eval rules live in
[AGENTS.md](AGENTS.md). Pull requests state the affected skills and any
activation or boundary change.

## Validate

```bash
pnpm run verify
```

CI runs the same gate. `pnpm exec oxfmt "**/*.md"` fixes Markdown formatting.
