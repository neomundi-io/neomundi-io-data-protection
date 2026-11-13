<div align="center">

<sub><b>English</b> · <a href="README_FR.md">Français</a></sub>

# Data protection — technical documentation **V0.1**

### What we verified, how we verified it, and what we did not.

</div>

---

> **Version 0.1 · 13 November 2026 · provisional.**
>
> 🔴 **This is not a privacy policy, not a DPA, and not a statement of GDPR
> compliance.** It is a technical description, dated and bounded, of how
> account data is erased and where content travels — with its limits named
> rather than omitted.

---

## Why a version 0.1

This documentation exists because the previous page in this repository
([`../../README.md`](../../README.md), dated 25 October 2026) stated plainly
that *"no universal retention duration or deployment-wide deletion guarantee
has been established by this documentation review."*

That is still true for the platform as a whole. What changed is that one
part of it — **account erasure** — has now been built, tested and reviewed in
depth. These pages describe that work, and only that work.

## Scope

| Covered | Not covered |
|---|---|
| Erasure of a portal account and what survives it | Any retention period — none has been decided |
| Which content leaves the machine, and to whom | The historical behaviour of the production service |
| The difference between NeoMundi-initiated and user-initiated calls | Station, Evidence, the public websites |
| What the code guarantees, and what it cannot | Which legal entity is the data controller |

## 🔴 Read this before anything else: merged is not deployed

The erasure behaviour described in
[`DATA_RETENTION.md`](DATA_RETENTION.md) was merged on **13 November 2026**
into the production lineage branch, at commit
[`1a64aeaf`](https://github.com/neomundi-io/Backend_plateforme_v2/commit/1a64aeaff073f633f06795d61cb8e8424b90299c)
*(private repository)*.

**It has not been deployed.** No image-publishing workflow listens to that
branch, and none was triggered. **The running service does not behave this
way yet.**

Every statement about erasure in these pages is therefore marked **merged,
not deployed**. We would rather publish that distinction than let a reader
assume the better behaviour is already live.

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
3. **No entity is named as data controller here**, because that decision is
   open. Without it, this cannot become a privacy policy.

## The four pages

| | |
|---|---|
| [`DATA_PROCESSING.md`](DATA_PROCESSING.md) | categories, purposes, flows, recipients |
| [`DATA_RETENTION.md`](DATA_RETENTION.md) | erasure, what survives, revocation of access |
| [`SUBPROCESSORS.md`](SUBPROCESSORS.md) | who receives what, and on whose instruction |
| [`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md) | what is open, and what we have not checked |

## Contact

For a question about this documentation, open an issue on this repository.

⚠ We do not publish a data-protection contact address here. Naming one
implies a designated controller and a commitment to answer within a
regulatory deadline — and the controller question is precisely what remains
open *(see [`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md))*. For account or
service matters, use the channels on your contract or in the service
interface.

---

<sub>V0.1 · 13 November 2026 · Apache-2.0. Earlier descriptions remain in Git
history and are not current commitments.</sub>
