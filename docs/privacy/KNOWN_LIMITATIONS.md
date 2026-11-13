<div align="center">

<sub><b>English</b> · <a href="KNOWN_LIMITATIONS_FR.md">Français</a></sub>

# Known limitations

</div>

> **V0.1 · 13 November 2026 · provisional.**
>
> This page exists because a documentation that lists only what works is a
> sales page. Everything below is open.

---

## 1. Merged is not deployed — the limitation that governs all the others

The erasure behaviour described in
[`DATA_RETENTION.md`](DATA_RETENTION.md) was merged on 13 November 2026 into
the production lineage branch. **It has not been deployed.**

🔴 **The running service still behaves as before**: an account deletion
cascades, and that cascade destroys invoices. Everything we describe as
verified is verified **on the merged candidate**, not in production.

**What would close this:** a deployment, which requires an explicit
authorisation and the checks in §2.

## 2. External transfers — not confirmed, not ruled out

The current code forbids sending to `openai` by default, and the guard is
covered by tests that run in continuous integration.

🔴 **This says nothing about the past.** Before the guard existed, the
document-grounding path was structurally armed, and the rejection of an
invalid credential would have arrived **after** the request body had already
left the machine.

Two checks against the production service would settle it. **Neither has
been carried out.** One counts, in the measurement store, how many requests
ever carried a grounding context — it is the only one that covers the whole
history of the corpus. The other reads the service log, and is bounded by
the fact that the log has no established rotation and the container was
recreated on 12 November 2026.

**Verdict: neither confirmed nor ruled out.** An invalid credential is not
evidence of absence, and we will not present it as such.

**What would close this:** running those two read-only checks.

## 3. The controller is settled; the contract is not yet aligned

✅ **Settled on 13 November 2026.** The controller is **NeoMundi Recherche**,
association governed by the French law of 1 July 1901, RNA `W563015824` —
see [`README.md`](README.md). Earlier versions of this page recorded the
question as open; it is not.

🔴 **What is still open is narrower, and contractual.** The hosting
agreement with Infomaniak was concluded under a **trading-style denomination
that is not a declared legal entity**, and carries **no legal identifier**.
The address on it is that of NeoMundi Recherche.

**We do not call that agreement void.** That would be a legal conclusion,
and we have not obtained one. What we state is narrower: the contractual
denomination and the designated controller are **not yet aligned**.

⚠ **A second limitation follows, and it is the one that matters to a
reader.** Naming a controller does not make these pages an information
notice under Articles 13 and 14 of the GDPR. **A privacy policy remains to
be written** — and that is now a legal drafting task, no longer a blocked
decision.

**What would close this:** a rectification or an amendment from the
provider naming NeoMundi Recherche, and a privacy notice drafted with legal
advice.

## 4. No accounting retention period

Accounting records are retained. **For how long is not decided**, and this
documentation will not invent a duration.

⚠ Historical invoices were issued under a denomination that is **not** the
designated controller. Those documents are **not rewritten** — an issued
accounting record is not edited after the fact. They are a historical
record, and they confer no current responsibility on the entity that
appears on them.

**What would close this:** a documented decision naming both the duration
and the entity that carries it.

## 5. Surfaces erasure does not reach

| Surface | Nature of the limit |
|---|---|
| **Measurement records** | retained deliberately; de-linked, **not anonymised** — still personal data |
| **Audit fingerprint** | **pseudonymisation, not anonymisation** — an unsalted digest of a low-entropy identifier |
| **Signed contracts** | structural: altering them destroys their evidential value |
| **Files outside the database** | reached by an operational procedure that is **not** part of this code |
| **Backups** | expire within 14 days; an erasure does not propagate into them before that |
| **A provider you chose** | outside our reach entirely — see [`SUBPROCESSORS.md`](SUBPROCESSORS.md) §3 |

**What would close these:** for the first two, a decision on whether the
retention basis remains acceptable; for the others, written procedures
rather than code.

## 6. Limits of the checks themselves

Being precise about what a verification does **not** establish:

- **Continuous integration ran on a candidate, not on production.** A green
  pipeline is evidence about code, not about a running system.
- **The tests use synthetic data.** No real account was erased, and no
  production database was touched, at any point.
- **The test environment is reproducible but not pinned.** Dependencies
  install from declared version ranges, without a lock file.
- **Three successive reviews each found a defect the previous one could not
  see** — by widening the perimeter from functions, to their effect in the
  database, to a real HTTP journey. That perimeter is now covered at all
  three levels. **This is not a claim that no defect remains.**

## 7. No overall GDPR compliance statement

🔴 **NeoMundi is not presented here as compliant with the GDPR**, and
nothing in these pages should be read as such a statement.

What this documentation establishes is narrower and, we think, more useful:
a dated, verifiable description of specific technical behaviours, with the
boundary of each one named.

Compliance is a legal assessment that depends on §3, §4 and §5 — none of
which engineering can close on its own.

---

## Reporting a problem

If you believe this documentation is inaccurate, open an issue on this
repository.

For a **security** matter, please do not open a public issue. Use your
contractual contact instead, and describe the problem without publishing
steps that would let someone reproduce it.

⚠ This organisation does not yet publish a coordinated-disclosure policy.
That is itself a gap, and it is listed here rather than left unsaid.
