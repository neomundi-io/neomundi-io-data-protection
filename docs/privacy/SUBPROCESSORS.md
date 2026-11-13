<div align="center">

<sub><b>English</b> · <a href="SUBPROCESSORS_FR.md">Français</a></sub>

# Recipients and sub-processors

</div>

> **V0.1 · 13 November 2026 · provisional.**
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

| Path | Default destination | Status |
|---|---|---|
| Document-grounding evaluators | **Cohere** 🇨🇦 / **OpenAI** 🇺🇸 | reachable; `openai` **is not allow-listed by default**; `cohere` is |
| Coherence, semantic risk, factuality evaluators | an **internal** model | **their address is a configuration variable** — a deployment can point them elsewhere |

⚠ **The guard protects against sending to *named providers*, not against
*arbitrary destinations*.** Three evaluator paths point at an internal model
by default, but nothing in the code prevents a configuration from making
them external. **Confirm the actual addresses for the deployment you are
reviewing rather than inferring them from this page.**

## 5. What we cannot tell you

🔴 **Whether content was transmitted to an external provider in the past is
neither confirmed nor ruled out.**

The allow-list described here is recent. Before it, the document-grounding
path was structurally armed toward a provider that is now excluded by
default. Establishing what actually happened requires checks against the
production service that have **not** been carried out.

**An invalid credential is not evidence that no transfer ever occurred.** We
will not make that inference, and nothing on this page should be read as
making it. See [`KNOWN_LIMITATIONS.md`](KNOWN_LIMITATIONS.md) §2.

## 6. Payments

A payment provider receives the account data strictly necessary to process a
transaction.

⚠ Payment collection is currently disabled on the platform. We mention the
provider because the integration exists, not to suggest it is in active use.
