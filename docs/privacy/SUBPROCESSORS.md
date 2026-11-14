<div align="center">

<sub><b>English</b> · <a href="SUBPROCESSORS_FR.md">Français</a></sub>

# Recipients and sub-processors

</div>

> **V0.5 · 14 November 2026 · provisional.**
>
> 🔴 **This page lists only what is established.** A provider that *could*
> be configured is not a provider that *is* receiving data, and we do not
> present one as the other.

---

## 1. Three categories, which must not be merged

| | |
|---|---|
| **A — Confirmed sub-processors** | a contract exists; they process on our instructions |
| **B — Providers you choose yourself** | you supply the credential and name the destination; **the relationship is yours** |
| **C — Configurable, not established as active** | the code can reach them; whether a given deployment does is a configuration question |

## 2. Category A — confirmed sub-processor

| Provider | Role | Location |
|---|---|---|
| **Infomaniak** | hosting of the service and its databases; outbound email; inference service used by the semantic-risk channel | 🇨🇭 Switzerland |

A data-processing agreement exists with this provider, concluded
electronically on **13 November 2026**, in which the client is named as
controller and Infomaniak Network SA as processor.

⚠ **One reservation, stated rather than hidden.** That agreement names the
client by a **trading-style denomination that is not a declared legal
entity**, and carries **no legal identifier** — no registration number, no
legal form. The address on it is that of NeoMundi Recherche.

🔴 **We do not conclude from this that the agreement is void** — that is a
legal assessment, and we have not obtained one. We conclude that the
contractual denomination and the designated controller **are not yet
aligned**, and a rectification has been prepared with the provider. See
[`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md) §3.

## 3. Category B — providers **you** choose

On `POST /v1/govern/stream`, you supply your own provider API key and name
the provider. We then call **that** provider, with **your** credential.

| | |
|---|---|
| **Who chooses** | you, on every request |
| **Whose credential** | yours |
| **Who holds the contract with them** | **you** |
| **Recognised destinations** | about a dozen, including providers in the 🇺🇸 United States and 🇨🇳 China |

🔴 **These are not our sub-processors.** We do not instruct them, we have no
agreement with them, and we have no visibility over what they do with your
content. **Transfer outside Switzerland and the EU on this path depends
entirely on your choice.**

Before using this path, review the terms of the provider you intend to name.

## 4. Category C — reachable by configuration, not established as active

Being honest about this category matters more than keeping it short.

| Path | Status, **measured on 13 November 2026** |
|---|---|
| **Cohere** 🇨🇦 — document grounding | 🔴 **moved to category B-bis: the transfer is confirmed.** 760 measurements, 4 pilot accounts, July→November 2026. See [`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md) §2 |
| **OpenAI** 🇺🇸 — document grounding | 🟢 **no result ever stored**, over 340,602 measurements. Not allow-listed by default, and the deployed guard refuses it |
| Coherence, semantic risk | 🔴 **actually point at our host's AI service** in production, not at an internal model |
| Factuality | 🔴 **also points at our host's AI service** in production. Version 0.2 of this page described this channel as unavailable, on the strength of a supervision probe; the probe was wrong. Re-measured on 13 November 2026: **1831 of 1841** measurements over seven days carry a measured factuality signal, `evaluator_runtime=api.infomaniak.com`, `served_by=primary` |

⚠ **The guard protects against sending to *named providers*, not against
*arbitrary destinations*.** An evaluator's address is a configuration
variable, and the deployed configuration is **not** the code default — as
the third row shows. **Read the actual addresses for the deployment you are
reviewing rather than inferring them from this page.**

## 5. What we now know, and what we still cannot tell you

Version 0.1 of this page said transfers were *"neither confirmed nor ruled
out"*. **The checks have since been carried out**, read-only, over the whole
history of the measurement store.

🔴 **One transfer is confirmed:** 760 measurements were grounded by Cohere
between July and November 2026, for 4 pilot accounts. §3 above and
[`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md) §2 carry the figures.

🟢 **For OpenAI, no grounding result was ever stored** — zero out of
340,602 measurements.

**What we still cannot tell you**, and will not pretend otherwise: whether a
request body ever left the machine toward a provider that then rejected it.
A rejected call leaves no record, and the service log only covers since
12 November 2026. **An invalid credential is not evidence that no transfer
ever occurred**, and nothing on this page should be read as making that
inference.

## 6. Payments

A payment provider receives the account data strictly necessary to process a
transaction.

⚠ Payment collection is currently disabled on the platform. We mention the
provider because the integration exists, not to suggest it is in active use.
