# Design

Use when a task changes how a put.io surface looks: a design canvas board, a
repository's `DESIGN.md`, or UI built to match either.

## Precedence

Three sources, highest first. When they disagree, the higher one wins and you
fix the lower one.

1. **[`@putdotio/design`](https://github.com/putdotio/putio-design)**: tokens,
   fonts, and icons for every platform. Values come only from its release. A
   value it lacks is a putio-design change first; the platform takes it from
   the next release.
2. **The repository's root `DESIGN.md`**: the platform's design contract. It
   records decisions; it never invents token values.
3. **The platform's canvas**: a claude.ai Design canvas with boards and
   reference captures. It is where options are explored. A choice on the
   canvas becomes binding only when it lands in `DESIGN.md`.

## Loop

1. **Capture** the shipped app through the repository's harness.
2. **Board** the change on the canvas, starting from those captures, with
   references beside it.
3. **Record** the chosen option in `DESIGN.md`, in the pull request that
   builds it or in a docs pull request before it.
4. **Build**, then compare proof captures against the board.

## `DESIGN.md`

- One per app repository, at the root, in the
  [design.md format](https://github.com/google-labs-code/design.md/blob/main/docs/spec.md):
  YAML front-matter tokens, then `##` sections in the spec's order. Product
  rules beyond the spec go in extra sections after them.
- Its tokens are generated from the locked `@putdotio/design` release and
  checked in the repository's gate, never typed by hand.
- `npx @google/design.md lint DESIGN.md` runs in the repository's verify gate.
- A UI change that changes the design updates `DESIGN.md` in the same pull
  request, unless an earlier docs pull request already recorded it.

## Canvases

The put.io Notion
[Design canvases](https://app.notion.com/p/403d546ba2f581b89776c1760cf77bae)
page indexes every canvas, its owner, and how the team works with it. Canvas
links are private: they live in Notion, never in a repository.

- Canvas edits need a harness with the claude.ai Artifact tool. Without one,
  name the board and the change for the user to make.
- Read the canvas right before each publish. Others edit it between your
  reads.
- Each canvas loads fonts and Phosphor from its own uploaded assets. Copy
  them between canvases server-side, then repoint the canvas's kit CSS to the
  new asset URLs.
- Reference captures of ecosystem apps stay on the canvas's reference pages,
  labeled with the app's name. They never enter a repository.
