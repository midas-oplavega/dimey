# Dimey

Dimey is an open source, privacy-first personal finance app. It reads bank
SMS and email alerts to record the money coming into and going out of your
accounts automatically, and lets you split expenses with friends in the same
app.

**Status:** design stage. There is no code yet, and contributions are not
being accepted at this stage.

## The problem

Money moves through many channels: several bank accounts, credit and debit
cards, UPI. Nothing shows in one place what came in and what went out.
Working that out usually means going through statements months later, when
nobody remembers what half the transactions were for.

Shared expenses are one part of this. They are often entered into a
splitting app months after they were paid, by which time everyone has
forgotten the details.

## Goals

- **Automatic capture.** Detect transactions from bank SMS and email alerts,
  so nothing has to be typed in.
- **A prompt at the moment of spending.** Notify as soon as a transaction is
  detected, with one tap to add a note or split it.
- **Multiple sources without duplicates.** Recognise when an SMS, an email
  and a statement line describe the same transaction.
- **Splitting built in.** Split any transaction with friends, track balances
  and settle up, including partial settlements.
- **Private by default.** The first release is local-only, and nothing leaves
  the device.

The first target is Android, with banks, cards and UPI in India.

## Privacy

The first release is local-only.

- Raw messages, transactions and account details never leave the device.
- Splits use local names for friends, and nothing is synced.

The design leaves room for a later cloud release, which is not scheduled. It
would be a separate, optional build that adds an account, sync between your
own devices, a website and splits shared with friends.

## Licence

AGPL-3.0-or-later. Copyright (C) 2026 Midas Oplavega. See [LICENSE](LICENSE).
