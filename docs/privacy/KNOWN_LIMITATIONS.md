<div align="center">

<sub><b>English</b> · <a href="KNOWN_LIMITATIONS_FR.md">Français</a></sub>

# Known limitations

</div>

> **V0.3 · 13 November 2026 · provisional.**
>
> This page exists because a documentation that lists only what works is a
> sales page. Everything below is open.

---

## 1. 🟢 Deployed — the limitation that governed all the others is closed

Versions 0.1 and 0.2 opened this page with the warning that the erasure
behaviour was merged but **not deployed**, and that the running service
still destroyed invoices on an account deletion.

**That is no longer the case.** Production serves revision `fc5fadd9d212…`
since 13 November 2026, verified on the running containers: the egress
guard refuses `openai`, the excerpt fallback returns an empty list, the
second on-disk copy is disabled, and invoice issuance refuses to run while
no issuer is decided.

⚠ **Two things this does NOT close**, and they are the subject of §3 and
§4: the invoice issuer is still undecided — which is precisely why issuance
now refuses rather than producing an incomplete document — and no
accounting retention period is set.

⚠ **And one statement that is still live and still wrong**: the service
terms served by the portal name an entity that no longer operates the
service. They were deliberately left untouched, because their text is
served from memory with no archive while each account records the version
it accepted — editing version 1.0 in place would destroy every existing
proof of consent. Issuing a corrected version is a legal drafting task, not
a deployment.

## 2. External transfers — measured, and one is confirmed

🟢 **The checks announced in version 0.1 have been carried out**, read-only,
against the production service and over the **whole** history of the
measurement store: **340,602 measurements, 1 May to 13 November 2026**.

Version 0.1 said this was *"neither confirmed nor ruled out"*. It is now
measured, and the two providers give **different** answers.

### Cohere — 🔴 transfer **confirmed**

| | |
|---|---|
| Measurements carrying a grounding result | **760** |
| Model that produced them | **`command-a-03-2025`** — a Cohere model, on all 760 |
| Window | **10 July → 4 November 2026** |
| Distinct customers | **4** |

🔴 **For a result to exist, the content had to be sent.** This is a fact,
not an inference. The channel is **declared** in
[`DATA_PROCESSING.md`](DATA_PROCESSING.md) §3.1 and it is **optional** — it
runs only when a request attaches documents for grounding.

⚠ **What remains open, and it is not technical:** no processing agreement
with this provider is on file. Canada benefits from a **partial** adequacy
decision, limited in scope; whether this provider falls inside it is a
**legal** assessment we have not obtained and will not improvise.

### OpenAI — 🟢 **absence established, on the verified perimeter**

| | |
|---|---|
| Measurements carrying an OpenAI grounding result | **0 out of 340,602** |
| Occurrences in the service log | **0** |

🔴 **Read that precisely.** What is established is that **no OpenAI
grounding result was ever stored**, over the entire corpus. What is **not**
established is that no request body ever left the machine: a call rejected
by the provider leaves no record, and the service log only covers since
**12 November 2026** — the container was recreated that day.

**An invalid credential is not evidence of absence**, and we do not present
it as such.

**What would close the remainder:** the egress guard reaching production —
see §1.

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
