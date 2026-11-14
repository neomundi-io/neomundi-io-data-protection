<div align="center">

<sub><b>English</b> · <a href="DATA_RETENTION_FR.md">Français</a></sub>

# Erasure and retention

</div>

> **V0.4 · 14 November 2026 · provisional.**
>
> 🟢 **Everything in sections 2 to 5 is DEPLOYED.** Production serves
> revision `fc5fadd9d212…` since 13 November 2026, verified on the running
> containers. Versions 0.1 and 0.2 of this page said otherwise, and said so
> deliberately — the behaviour was then merged but not live.

---

## 1. The principle: three tiers, not one operation

Deleting an account used to be indivisible: one `DELETE`, and a cascade took
everything with it — including **invoices**.

That is wrong in both directions. It destroyed records the law requires to
be kept, and the first attempt to fix it went too far the other way: it
*refused the whole erasure* whenever an invoice existed, which made **every
account that had ever been invoiced impossible to erase**. A right that is
never exercised does not exist.

🔴 **The two goals stop competing once you notice one fact: an invoice
carries its own copy of the customer's name, address, VAT number and email,
frozen at issuance. The accounting record is the invoice, not the account
row.**

Emptying the account row therefore takes nothing away from accounting — and
not deleting it stops the cascade.

| Tier | Treatment | Basis |
|---|---|---|
| **Personal data** | **erased, always** | nothing requires retention |
| **Accounting records** | **retained, intact** | legal retention obligation |
| **Measurement results** | **retained, de-linked** | documented scientific purpose |

## 2. What is erased

| Data | Treatment |
|---|---|
| Name, surname, postal address, postcode, city | overwritten with a constant marker |
| Company, VAT number, phone, job title, free-text note | set to null |
| **Password hash** | overwritten — **no password authenticates afterwards**, measured |
| Email | replaced by an unusable per-row address |
| **Referral code** | released — it is unique, and often contains a first name |
| **API keys** | **deleted — including revoked ones** |
| **Usage events** | deleted |
| **Password-reset tokens** | deleted |
| Promotional-code uses | deleted |
| Pending registration record | anonymised, and its link to the account cut |

🔴 **Sessions are refused.** Two independent locks: a status check on every
bearer path *and* on token refresh, backed by the database; and invalidation
of already-issued tokens, applied best-effort so that an infrastructure
outage can never block an erasure.

**Why two locks.** While erasure deleted the account row, every
authentication boundary held for one implicit reason — the account simply
was not there. Keeping the row to protect accounting removes that safety net
everywhere at once. We found this with an end-to-end test: a refresh token
issued before the purge still returned a **new access token**. It never
reached production.

**Guarantees verified by test:** idempotent · transactional *(a fault
injected mid-operation writes nothing)* · a replayed purge is refused · the
original email address becomes available again.

## 3. What is retained, and why

### 3.1 Accounting records

Invoices, wallets and their transactions, bank transfer requests, affiliate
commissions and withdrawals are **kept intact**.

🔴 **No retention period is stated here, because none has been decided.** We
will not invent one. Until that decision is recorded, the records are kept —
keeping is reversible, destroying is not.

### 3.2 Measurement results

Measurement results are **not** deleted with an account. Destroying them
would destroy the corpus the measurements exist to constitute.

What changed is the link: the registration record is anonymised and its
account reference cleared, so the join that used to lead from a retained
excerpt back to a named person is broken.

⚠ **This is de-linking, not anonymisation.** The records remain personal
data under the GDPR, retained on a documented basis.

## 4. Excerpts of your response

A measurement may store **designated** excerpts of the response — at most
**10**, of at most **200 characters** each. Never the prompt. Never the
whole response.

A mechanical fallback used to copy the **first three sentences of the
response** whenever the evaluator had designated nothing. On 927 real
responses, that fallback produced **69.6%** of all stored excerpts. **It has
been removed.** The opening of an answer is not a suspicious passage; it is
just its opening. *Deployed and verified on 13 November 2026.*

🔴 **Excerpts already stored have not been altered.** Removing them is a
separate decision, not taken here, and a naive deletion would destroy the
provenance that makes a measurement re-checkable.

## 5. The fingerprint in the audit log — pseudonymisation, not anonymisation

The erasure writes a one-way fingerprint of the address into the audit log,
so that "did you erase alice@example.com?" can be answered later without
keeping the address.

🔴 **This is pseudonymisation. It is not anonymisation, and we prefer to say
so.** It is an **unsalted** SHA-256 of an email address. Addresses are
low-entropy: anyone holding a list of addresses can recover this one by
comparison, and the domain is stored beside it, which narrows the search
further. **Under the GDPR this fingerprint remains personal data.**

The choice is accepted, not overlooked: salting or keying it would make the
fingerprint unverifiable by the one person entitled to ask — which is
exactly what it is for.

## 6. What erasure does **not** reach

| Surface | Why |
|---|---|
| **Measurement records** | retained deliberately, de-linked — §3.2 |
| **The audit fingerprint** | pseudonymised, retained — §5 |
| **Signed contracts** | altering them would invalidate the signature |
| **Files outside the database** | reached by an operational procedure, not by this code |
| **Database backups** | they expire on their own, **within 14 days** |
| **A provider you chose yourself** | the relationship is yours — see [`SUBPROCESSORS.md`](SUBPROCESSORS.md) |

🔴 **We do not claim erasure is complete.** Six surfaces fall outside what
the code can guarantee, and listing them is more useful than a sentence that
implies otherwise.
