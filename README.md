# Dimey

Dimey is an open source, privacy-first expense tracker that records
transactions automatically from bank SMS and email alerts, and lets you split
them with friends in the same app.

**Status:** design stage. There is no code yet, and contributions are not
being accepted at this stage.

## The problem

Money moves through many channels: several bank accounts, credit and debit
cards, UPI. Working out where it went usually means going through statements
months later, when nobody remembers what half the transactions were for.

Shared expenses have the same problem. The dinner is paid for today and
entered into a splitting app months later, by which time everyone has
forgotten the details.

Both come from the same gap: the expense is recorded long after the money is
spent.

## Goals

- **Automatic capture.** Detect transactions from bank SMS and email alerts,
  so nothing has to be typed in.
- **A prompt at the moment of spending.** Notify as soon as a transaction is
  detected, with one tap to add a note or split it.
- **Multiple sources without duplicates.** Recognise when an SMS, an email
  and a statement line describe the same transaction.
- **Splitting built in.** Split any transaction with friends, track balances
  and settle up, including partial settlements.
- **Private by default.** Everything stays on the device unless you choose to
  share a split.

The first target is Android, with banks, cards and UPI in India.

## Privacy

- Raw messages, transactions and account details never leave the device.
- A split can be kept private, using local names for friends, with nothing
  synced.
- Sharing splits with friends needs an account and a server. Only the shared
  split and settlement records are stored there.

## Licence

AGPL-3.0-or-later. Copyright (C) 2026 Midas Oplavega. See [LICENSE](LICENSE).
