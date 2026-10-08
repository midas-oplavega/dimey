# 1. Flutter with a pure Dart core

- **Status:** accepted
- **Date:** 2026-10-08

## Context

Dimey targets Android first, then iOS and a website. Four requirements shape
the choice of language and UI toolkit.

- **Core logic written once.** Parsing bank messages into transactions,
  detecting duplicates, split and balance arithmetic and the data model are
  the same on every platform. Platform-specific code is limited to adapters
  around that core.
- **Two releases.** A local-only release keeps all data on the device. A
  cloud release can sync data between a user's devices and is the only
  release with a website.
- **SMS capture exists only on Android.** iOS and web browsers give apps no
  access to SMS. The SMS adapter is therefore Android-only, and it has to run
  when the app is closed.
- **The website is a full client.** It does not capture SMS, but it imports
  bank statements and shows and settles splits, so the core runs in the
  browser.

## Decision

Dimey is built with Flutter, with the core and the UI written in Dart.

- **The core is a pure Dart package.** It does not depend on Flutter or on
  any platform library. It defines interfaces for what it needs from a
  platform: message sources, storage, notifications and sync.
- **Adapters implement those interfaces per platform.** The UI and the
  adapters depend on the core. The core depends on neither.
- **The Android SMS adapter is written in Kotlin.** It receives the SMS and
  hands it to the Dart core.
- **The two releases are builds of one codebase.** They differ only in the
  adapters they include. The local-only build includes no sync adapter.

## Options considered

- **Flutter (Dart).** Chosen. One language and one toolkit cover the core and
  the UI on Android, iOS and the web, and the core compiles to run in the
  browser.
- **Kotlin Multiplatform with Compose Multiplatform.** The best fit for the
  SMS adapter, because Android code would call a Kotlin core directly.
  Rejected because the website is a full client and web is the least mature
  Compose Multiplatform target: the Kotlin documentation lists it as Beta,
  and Android and iOS as Stable.
- **React Native with Expo (TypeScript).** The strongest web target.
  Rejected because it brings the most tooling of the three, a JavaScript
  toolchain on top of the native builds, and gains nothing on the hardest
  feature: background SMS capture still needs Kotlin plus a background
  JavaScript task.

## Consequences

- **One language for most of the code.** The core, the UI and their tests
  are Dart on every platform.
- **Core tests need no device.** Because the core does not depend on
  Flutter, its tests run on a development machine without a phone or an
  emulator.
- **Kotlin is still required.** The SMS adapter is native Android code, and
  it must start the Dart core in the background when a message arrives while
  the app is closed. This bridge is the most delicate platform code in the
  project.
- **The website is an app in the browser.** Flutter draws the page itself
  instead of producing ordinary HTML, so the first load is heavier than a
  conventional site and the result is unsuited to content pages.
- **iOS builds need macOS.**
- **Dependencies come from the Dart ecosystem.** Each one, including the
  library that extracts text from statement PDFs, needs a licence compatible
  with AGPL-3.0-or-later.

The local database, the server and the sync design are separate decisions.
