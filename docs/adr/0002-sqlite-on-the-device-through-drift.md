# 2. SQLite on the device, through drift

- **Status:** accepted
- **Date:** 2026-10-08

## Context

[ADR 1](0001-flutter-with-a-pure-dart-core.md) puts storage behind an
interface that the core defines. This ADR chooses what implements that
interface on the device. Five requirements shape the choice.

- **The data is relational.** Transactions, accounts, friends, splits and
  settlements refer to one another, and balances and duplicate detection are
  queries across them.
- **One schema on every platform.** Android, iOS and the website store the
  same data, so the database has to run on all three, including in the
  browser.
- **Background writes.** On Android the SMS adapter writes to the database
  from the background, independently of the UI.
- **Sync in the cloud release.** Records created on different devices are
  merged, so the schema has to allow for that from the first release.
- **An open licence.** Every dependency needs a licence compatible with
  AGPL-3.0-or-later.

## Decision

The database on the device is SQLite on every platform, accessed through the
drift library.

- **drift is a detail of the storage adapter.** The core does not depend on
  drift or on SQLite. It sees only its own storage interface.
- **Amounts are integers in the currency's minor unit**, such as paise for
  rupees. Floating point is not used for money.
- **Record IDs are UUIDs generated on the device**, not auto-incremented
  integers, so records created on two devices cannot collide.

## Options considered

- **SQLite through drift.** Chosen. SQLite is relational, in the public
  domain and runs on Android, iOS and in the browser. drift adds SQL that is
  checked at build time with typed results, versioned schema migrations with
  generated tests, and support for all three platforms. It is plain Dart
  under the MIT licence, so the storage adapter is tested on a development
  machine against an in-memory database.
- **SQLite without a library layer.** Workable, but row mapping, migrations
  and the browser setup would be written by hand.
- **Key-value and object stores** such as Hive, Isar and ObjectBox. Rejected
  because the data is relational and balances are aggregate queries.
- **Bundled sync platforms** such as Firebase, Supabase and PowerSync.
  Rejected because each fixes the server technology and by default stores
  data the server can read, which would settle the server and the privacy
  model as a side effect of choosing a database.

## Consequences

- **Background writes do not refresh the UI by themselves.** An SMS that
  arrives while the app is closed is handled in a separate background engine
  with its own connection to the database file. SQLite keeps the file
  consistent across connections, but drift's live queries do not see writes
  made through another connection, so the app has to refresh them
  explicitly.
- **The website ships SQLite itself.** It serves a WebAssembly build of
  SQLite and a drift worker script. The fastest storage mode needs the
  `Cross-Origin-Opener-Policy` and `Cross-Origin-Embedder-Policy` response
  headers, which drift's documentation says conflict with some pop-up
  sign-in flows. Without them drift falls back to slower storage.
- **The database file can be encrypted on Android and iOS** through an
  encrypted SQLite build. An existing unencrypted database can be converted,
  so encryption can be introduced after the first release. drift's
  documentation does not cover encryption in the browser.
- **Domain types are mapped.** Because the core does not depend on drift,
  the storage adapter converts between drift's generated row types and the
  core's own types.
- **The build has a code generation step.** drift generates Dart code from
  the schema and the SQL.

This ADR covers the database on the device. The server's database, the sync
design and whether to encrypt the database file are separate decisions.
