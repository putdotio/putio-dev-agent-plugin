# Examples

Copy the shape of these examples, not the implementation.

## `taizn`

`taizn` demonstrates the packaging, install, and signing side of a frontend surface harness.

Pattern signals:

- tiny TypeScript CLI companion for Tizen work
- wraps Tizen CLI and `sdb`
- exposes focused commands such as `profile`, `package`, and `install`
- uses a repo config file for stable inputs
- uses an ignored working directory for local output and generated state
- reads sensitive values from env or local config instead of git
- turns multi-step Tizen ceremony into repeatable commands

Use this pattern when a platform needs reliable build/package/install setup before flow testing can start.

Good boundaries:

- keep signing material, cert passwords, device identities, and local SDK paths out of checked-in examples
- keep app-specific navigation and playback checks out of the generic Tizen wrapper
- make packaging and install failures typed enough that an agent can fix the root cause

## `putio-roku`

`putio-roku` demonstrates a hardware-backed product-flow proof loop.

Pattern signals:

- `pnpm roku` scripts wrap common live-device workflows
- TypeScript scripts drive live Roku checks through `@putdotio/rokit`, which
  wraps Roku ECP for launch, deeplinks, keypresses, and screenshots
- SceneGraph/runtime queries support assertions when available
- screenshots, logs, review HTML, and compact summaries make behavior reviewable
- live checks cover auth state, deeplink navigation, playback, focus, media-key behavior, and visual artifacts

Use this pattern when platform behavior can only be trusted after running on a real device or emulator.

Good boundaries:

- keep Roku IPs, developer passwords, channel IDs, account data, and content IDs out of git
- keep generic remote-control and screenshot primitives separate from app journeys
- keep scenario expectations in named scripts or scenario files so the platform driver remains reusable
