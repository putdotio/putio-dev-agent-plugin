# Platform notes

Starting points only. Verify current platform tooling in the target repo before implementing.

Good proof usually shows the surface installed or booted on the selected
target and launched foregrounded, with a screenshot plus a log, trace, or state
excerpt saved with the summary. App-level harnesses also assert the expected
route or screen after a deeplink, launch argument, or entrypoint. Platform
alternatives and extra checks are noted below.

## Web and browser extensions

Likely tools:

- Playwright or repo-local browser automation
- extension pack/load tooling
- dev server or preview server commands
- browser logs, screenshots, traces, and network captures

Common harness concerns:

- dev server boot readiness
- authenticated session setup without using the human's browser profile
- route, deeplink, and extension entrypoint coverage
- extension permissions, service worker lifecycle, and content-script injection state
- reliable screenshots and traces for visual or startup failures

## Android and Android TV

Likely tools:

- Gradle or repo-local build commands
- Android `adb`, UI Automator, accessibility dumps, logcat, screencap, and
  screenrecord
- emulator manager, hardware, or cloud-device provider CLIs

Common harness concerns:

- emulator boot readiness and device selection when multiple targets are
  attached
- package install, activity launch, deeplinks, intents, and launch arguments
- app reinstall wiping local session state
- auth/session seeding through approved development hooks, checked before the product flow starts
- logcat noise filtering; logs, screenshots, and accessibility state that
  explain failures
- on TV, focus navigation with D-pad keys

## iOS and tvOS

Likely tools:

- Xcode command-line tooling, `simctl`, XCTest, or UI test runners
- repo-local build scripts for signing and provisioning
- simulator, hardware, or cloud-device provider CLIs

Common harness concerns:

- simulator boot readiness and simulator versus hardware support
- provisioning, signing, and team IDs
- install, launch, deeplink, and launch-argument support
- app reinstall wiping local session state
- auth/session seeding through approved development hooks, checked before the product flow starts
- logs, screenshots, and accessibility state that explain failures
- on tvOS, remote/button events through XCTest or simulator tooling, and
  video/playback assertions that may need app test hooks

Keep signing identities, provisioning profiles, device identifiers, and account facts outside git.

## Tizen

Likely tools:

- Tizen CLI
- `sdb`
- certificate/profile tooling

Common harness concerns:

- signing profiles and certificate passwords
- device discovery and developer mode
- packaging paths and app identifiers
- install and launch commands that fail with terse vendor output
- logs through `sdb`
- screenshots when the device/toolchain supports them
- a packaging and install wrapper proves the package artifact, install on the
  selected device, foreground launch, and startup logs through `sdb`; a
  screenshot or runtime signal confirms visibility when the device supports it,
  and app navigation stays in the app harness

## Roku

Likely tools:

- `@putdotio/rokit`
- Roku External Control Protocol
- SceneGraph or app debug query surfaces when available

Common harness concerns:

- device IP and developer password stay local
- sideload packaging and install state
- launch parameters and deeplinks
- keypress timing
- focus and playback state
- screenshots and review HTML for visual proof
- proof includes a keypress sequence reaching the expected focus and playback responding to media keys

## Other platforms

For webOS, game consoles, set-top boxes, browser-kiosk environments, or vendor
clouds, start from the same [harness pattern](./pattern.md)
layers. If the platform cannot expose runtime state, say so and design around
stronger visual, log, or transcript evidence.
