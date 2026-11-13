<div align="center">

<sub><b>English</b> · <a href="DATA_PROCESSING_FR.md">Français</a></sub>

# Data processing — categories, purposes, flows

</div>

> **V0.1 · 13 November 2026 · provisional.** Read
> [`README.md`](README.md) first — in particular the distinction between
> **merged** and **deployed**.

---

## 1. Categories

| Category | What it is | Where it lives |
|---|---|---|
| **Account data** | email, name, postal address, VAT number, phone, language | portal database |
| **Credentials** | password hash, API keys, reset tokens | portal database |
| **Submitted content** | the prompt, the response, and any documents attached for grounding | see §3 — handling differs per path |
| **Measurement results** | scores, decisions, reasoning, and **designated excerpts of the response** | measurement database |
| **Accounting records** | invoices, wallets, transactions, transfers | portal database |
| **Technical logs** | request identifiers, timings, error states | server logs |

🔴 **Measurement results are not purely numeric.** When the factuality
evaluator designates a passage as suspect, that passage is stored verbatim,
bounded to **10 excerpts of at most 200 characters**.

⚠ A measurement on 927 real responses found that **720 of them carried such
excerpts**, and that **69.6%** of those excerpts came from a mechanical
fallback that copied the first three sentences of the response whenever the
evaluator had designated nothing. **That fallback has been removed** — see
[`DATA_RETENTION.md`](DATA_RETENTION.md) §4. **Merged, not deployed.**

## 2. Purposes

| Purpose | Category |
|---|---|
| Operating the measurement service | submitted content, account data |
| Authentication and quota enforcement | credentials, account data |
| Billing | account data, accounting records |
| **Scientific qualification of the measurements** | measurement results |
| Security and abuse investigation | technical logs |

The scientific purpose is the reason measurement results are **not** deleted
with an account. See [`DATA_RETENTION.md`](DATA_RETENTION.md) §3.

## 3. Flows — the distinction that matters most

🔴 **Two very different things are sometimes called "sending data to a
provider". They must not be read as one.**

### 3.1 Calls **NeoMundi** initiates, with **NeoMundi's** credentials

To measure, the service may call an evaluator. Some evaluators run on
infrastructure we control; some do not.

| Channel | Where it runs | Content leaves? |
|---|---|---|
| Stability | our servers | no |
| Coherence | model on our own servers | no |
| Semantic risk | our host's inference service | yes — to our host |
| Factuality / instability | inference service | **channel unavailable at the time of writing** |
| **Document grounding** *(optional)* | **providers outside Switzerland and the EU** | **yes** |

**A guard now governs these calls.** It combines an explicit allow-list with
a credential plausibility check, and it is applied to **four** egress paths.
`openai` **is not on the allow-list by default**.

⚠ The fourth path — the demonstration endpoint — was not found by reading
the code. It was found by a **structural test** that parses the source and
flags any place where the mere presence of a credential is treated as
permission to send. *Merged, not deployed.*

### 3.2 Calls **you** initiate, with **your** credentials

On `POST /v1/govern/stream` you supply **your own provider API key**.
NeoMundi then calls **the provider you named**, with **your** key, and
measures the response as it streams.

| | |
|---|---|
| **What is sent** | your prompt, and any context you attach |
| **To whom** | **the provider you name** — about a dozen are recognised, located notably in the 🇺🇸 United States and 🇨🇳 China |
| **With which key** | **yours** |
| **Who decides** | **you**, on every request |

🔴 **The allow-list of §3.1 does not apply here, deliberately.** It governs
calls *we* initiate with *our* credentials. It has no business arbitrating a
call you asked for, to a provider **you** hold the contract with.

**What follows:** for this path, transfer outside Switzerland and the EU
**depends on your choice of provider**, and the relationship with them is
**yours**. We have neither a contract with them nor visibility over what
they do with your content.

### 3.3 A boundary worth stating plainly

The guard in §3.1 protects against sending to **named providers**. It does
not constrain **arbitrary destinations**: three evaluator paths point at an
internal model **by default**, but their address is a configuration
variable. **A configuration can make them external without any code
change.** Anyone reviewing a specific deployment should confirm those
addresses rather than infer them.

## 4. Recipients

See [`SUBPROCESSORS.md`](SUBPROCESSORS.md).

## 5. What we do not state here

🔴 **No legal entity is named as data controller in this documentation.**
That decision is open, and naming one before it is settled would be worse
than naming none. See [`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md) §3.

Consequently these pages **cannot** serve as the information notice required
of a controller. They describe the technical reality; the legal notice is a
separate document that does not yet exist.
