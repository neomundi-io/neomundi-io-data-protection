<div align="center">

<sub><b>English</b> · <a href="DATA_PROCESSING_FR.md">Français</a></sub>

# Data processing — categories, purposes, flows

</div>

> **V0.5 · 14 November 2026 · provisional.** Read
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
[`DATA_RETENTION.md`](DATA_RETENTION.md) §4. **Deployed on 13 November
2026** — the field now returns an empty list at every score.

## 2. Purposes

| Purpose | Category |
|---|---|
| Operating the measurement service | submitted content, account data |
| Authentication and quota enforcement | credentials, account data |
| Keeping pre-existing accounting records | account data, accounting records |
| **Scientific qualification of the measurements** | measurement results |
| Security and abuse investigation | technical logs |

⚠ **No invoicing is currently carried out.** The purpose above is the
*retention* of accounting records issued earlier, not an active billing
activity: since 13 November 2026 the running service **refuses** to issue an
invoice while no issuer is settled, rather than emitting an incomplete
accounting document.

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
| Coherence | 🔴 **our host's inference service** — version 0.4 of this page said "model on our own servers" and "no"; the deployed configuration says otherwise | **yes — to our host** |
| Semantic risk | our host's inference service | yes — to our host |
| Factuality / instability | our host's inference service | yes — to our host |
| **Document grounding** *(optional)* | **providers outside Switzerland and the EU** | **yes** |

⚠ **Where each evaluator actually runs, re-read on 14 November 2026.** Three
of the four channels above are served by our host's inference service, and the
local inference container declared in our deployment file has been **stopped
since 9 November 2026**. We publish this because a reader cannot verify it, and
because version 0.4 of this page got the coherence row wrong.

**A guard now governs these calls.** It combines an explicit allow-list with
a credential plausibility check, and it is applied to **four** egress paths.
`openai` **is not on the allow-list by default**.

⚠ The fourth path — the demonstration endpoint — was not found by reading
the code. It was found by a **structural test** that parses the source and
flags any place where the mere presence of a credential is treated as
permission to send. **Deployed on 13 November 2026**, and verified on the
running service: presented with a well-formed `openai` credential, it
refuses, reason `provider_not_allowed`. The allow-list actually served is
`['cohere']`.

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

### 3.3 What the deployed configuration actually does

Version 0.1 said three evaluator paths pointed at an internal model *"by
default"*, and that their address was a configuration variable. That was
accurate and **insufficient**: we have since read the deployed
configuration, and the default is not what runs.

| Channel | Where it actually points, in production |
|---|---|
| **Coherence / embeddings** | 🔴 **our host's AI service** — `api.infomaniak.com` |
| **Semantic risk** | 🔴 **our host's AI service** — `api.infomaniak.com` |
| Factuality | 🔴 **our host's AI service** — `api.infomaniak.com`. Version 0.2 of this page said this channel was unavailable, on the strength of a supervision probe that was wrong |

🔴 **So all three leave the machine**, toward the one provider we do
have an agreement with. Versions 0.2 to 0.4 of this page said *"two of the
three"*, because they counted the factuality channel as unavailable. It is
not. The destination is a **configuration variable**, not
a code constant: the guard in §3.1 protects against sending to **named
providers**, not against **arbitrary destinations**.

**Anyone reviewing a specific deployment should read those addresses rather
than infer them from this page** — including this one, which describes the
deployment measured on 13 November 2026.

## 4. Recipients

See [`SUBPROCESSORS.md`](SUBPROCESSORS.md).

## 5. Who answers for this processing

**NeoMundi Recherche**, association governed by the French law of
1 July 1901, RNA `W563015824`, 29 rue Tronchet, 75008 Paris —
see [`README.md`](README.md).

That covers the processing described on this page, **within the limits of
what is actually carried out**. It does not extend to the calls described in
§3.2, which you initiate with your own credentials toward a provider you
hold the contract with.

🔴 **Naming a controller is not an assessment of lawfulness.** These pages
describe technical reality; they are **not** the information notice a
controller owes under Articles 13 and 14 of the GDPR, and they do not claim
to be. That notice is a separate document, and writing it is not an
engineering task. What remains open is listed in
[`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md).
