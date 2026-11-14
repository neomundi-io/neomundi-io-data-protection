<div align="center">

<sub><b>English</b> · <a href="README_FR.md">Français</a></sub>

# Data protection — technical documentation **V0.4**

### What we verified, how we verified it, and what we did not.

</div>

---

> **Version 0.2 · 13 November 2026 · provisional.**
>
> 🔴 **This is not a privacy policy, not a DPA, and not a statement of GDPR
> compliance.** It is a technical description, dated and bounded, of how
> account data is erased and where content travels — with its limits named
> rather than omitted.

---

## What changed in version 0.2

Three things, all of them measurements rather than rewording:

1. 🟢 **The controller is settled** — NeoMundi Recherche, named above.
   Version 0.1 recorded that question as open.
2. 🔴 **The transfer checks were carried out.** One transfer is now
   **confirmed** (Cohere, 760 measurements, 4 pilot accounts); for the other
   provider, no stored result exists over 340,602 measurements. See
   [`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md) §2.
3. 🔴 **Two evaluator channels are not internal**, as version 0.1
   implied they were by default: in production they point at our host's
   AI service. Corrected in
   [`DATA_PROCESSING.md`](DATA_PROCESSING.md) §3.3.

🟢 **And in version 0.3, the one thing that had not changed, has.** The
erasure behaviour is **deployed**. Production serves revision
`fc5fadd9d212…`, verified on the running containers — see below.

## Why this documentation exists

This documentation exists because the previous page in this repository
([`../../README.md`](../../README.md), dated 25 October 2026) stated plainly
that *"no universal retention duration or deployment-wide deletion guarantee
has been established by this documentation review."*

That is still true for the platform as a whole. What changed is that one
part of it — **account erasure** — has now been built, tested and reviewed in
depth. These pages describe that work, and only that work.

## Data controller

**NeoMundi Recherche** — association governed by the French law of
1 July 1901.

| | |
|---|---|
| **RNA** | `W563015824` |
| **Registered office** | 29 rue Tronchet, 75008 Paris, France |
| **Published identity** | [neomundi.org/mentions-legales](https://neomundi.org/mentions-legales) |
| **To exercise your rights** | the contact form at [neomundi.org/contact](https://neomundi.org/contact) — no email address is published |

NeoMundi Recherche carries responsibility for NeoMundi's processing — the
measurement API, the portal, user accounts, keys, measurements, Station,
Evidence and the NeoMundi websites — **within the limits of the processing
actually carried out**.

🔴 **Naming a controller does not make the processing compliant.** It
settles who answers for it. What remains open is listed in
[`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md), and it is still substantial.

## Scope

| Covered | Not covered |
|---|---|
| Erasure of a portal account and what survives it | Any retention period — none has been decided |
| Which content leaves the machine, and to whom | The historical behaviour of the production service |
| The difference between NeoMundi-initiated and user-initiated calls | Station, Evidence, the public websites |
| What the code guarantees, and what it cannot | Whether the processing is lawful — naming a controller is not an assessment |

## 🟢 Deployed on 13 November 2026 — and verified on the running service

Versions 0.1 and 0.2 of this page said the erasure behaviour was *merged,
not deployed*, and warned readers not to assume the better behaviour was
live. **It is now live.**

| | |
|---|---|
| **Revision served** | `fc5fadd9d21251712ed34a13c23803dfccc37123` |
| Services | the measurement engine and the portal |
| Verified | on the running containers, not inferred from a branch |
| Unchanged | the web frontend, deliberately — this release does not touch it |

**What was verified on the running service**, not on a candidate:

- the external-egress guard is active, and refuses `openai` — `provider_not_allowed`, even when presented with a well-formed credential;
- the mechanical excerpt fallback returns an empty list at every score;
- the second on-disk copy of client output is disabled;
- the purged-account state exists, and invoice issuance refuses to run while no issuer is decided;
- `measurement_version` is **3.5.0**, unchanged — results stay comparable with everything measured before;
- the corpus, invoices, wallets, accounts and API keys are untouched, to the row.

## What is actually verified

| Verified | How |
|---|---|
| Account identity is overwritten field by field | a test sweeps **every** attribute of the row for sentinel values, not the fields it expects |
| API keys are deleted — including revoked ones | SQL assertions against a real PostgreSQL |
| Password-reset tokens do not survive | idem |
| Sessions are refused after erasure | a full HTTP journey: create → key → invoice → request → purge → **attempt to reuse the old tokens** |
| The operation is idempotent and transactional | a fault is injected mid-operation; nothing is written |
| Accounting records survive intact | the invoice is read back and is still legible without the account |

All of the above ran green in continuous integration: **198 portal tests**
and **1800 measurement tests**, zero failures.

## What we got wrong, and fixed

Three successive reviews each found a defect the previous one could not see.
We record this because the pattern is more useful than the fixes.

| Review perimeter | What it found |
|---|---|
| functions, in memory | — |
| **+ effect in the database** | keeping the account row cancelled a cascade: a purged account kept an **active API key** |
| **+ a real HTTP journey** | a refresh token issued before the purge still returned a **new access token** |

🔴 **The defect was never in the code being looked at.** It was in what the
change had removed *elsewhere*. None of these ever reached production.

## Limits

Named in full in [`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md). The three
that matter most:

1. **No retention period has been decided** for accounting records. We will
   not invent one.
2. **Whether any content was transmitted to an external provider in the
   past is neither confirmed nor ruled out.** The current code forbids it by
   default; that says nothing about history.
3. **The hosting agreement was signed under a denomination that is not a
   declared legal entity.** The controller is now settled; aligning the
   contract with it is not, and a rectification has been prepared.

## The four pages

| | |
|---|---|
| [`DATA_PROCESSING.md`](DATA_PROCESSING.md) | categories, purposes, flows, recipients |
| [`DATA_RETENTION.md`](DATA_RETENTION.md) | erasure, what survives, revocation of access |
| [`SUBPROCESSORS.md`](SUBPROCESSORS.md) | who receives what, and on whose instruction |
| [`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md) | what is open, and what we have not checked |

## Contact

**To exercise a right over your personal data**, use the contact form at
[neomundi.org/contact](https://neomundi.org/contact). It is the channel
NeoMundi Recherche publishes; **no email address is published**, and we do
not invent one here.

For a question about **this documentation**, open an issue on this
repository. For account or service matters, use the channels on your
contract or in the service interface.

---

<sub>V0.4 · 14 November 2026 · Apache-2.0. Earlier descriptions remain in Git
history and are not current commitments.</sub>
