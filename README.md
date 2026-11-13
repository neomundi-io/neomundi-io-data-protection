<div align="center">

<sub><b>English</b> · <a href="README_FR.md">Français</a></sub>

# NeoMundi — Data & Confidentiality

### Understand data flows before integrating AI.

**Which content moves, where measurements run, and what gets recorded or exported: the documented boundaries of the NeoMundi infrastructure.**

**Measurement → Station → Evidence**

[![Measurement](https://img.shields.io/badge/Measurement-Measure-2563eb?style=flat-square)](https://github.com/neomundi-io/neomundi-runtime-measurement)
[![Station](https://img.shields.io/badge/Station-Observe-0f766e?style=flat-square)](https://github.com/neomundi-io/neomundi-station-public)
[![Evidence](https://img.shields.io/badge/Evidence-Verify-D4A545?style=flat-square)](https://github.com/neomundi-io/neomundi-evidence-public)
[![Use cases](https://img.shields.io/badge/Use_cases-Explore-64748b?style=flat-square)](https://github.com/neomundi-io/neomundi-use-cases)

**[API](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/QUICKSTART.md)** · **[Create an account](https://controltower.neomundi.io/welcome)**

</div>

---

## One principle: describe each flow, not a blanket promise

NeoMundi distinguishes **transmission**, **processing**, **retention** and **export**. Content absent from an export may still have been transmitted, processed or stored on another path. This page makes no platform-wide promise of zero transmission, zero storage or zero retention.

| Component | Boundary to examine |
|---|---|
| **Measurement / API** | Prompts and responses may be submitted depending on the request; enabled evaluators may receive content for selected measurements. |
| **Station** | The internal ledger and read-only interface expose different data. A restricted interface does not establish absence of content in the ledger. |
| **Evidence** | Integrity verification, attestations and traceability do not themselves establish confidentiality or retention guarantees. Exported artefact contents require separate scrutiny. |

**Before integration:** confirm actual payloads, destinations, evaluators, logs, backups, retention periods and access controls for the selected deployment. Verify each provider's obligations separately.

[AI Act & GDPR technical contributions](https://github.com/neomundi-io/ai-act-rgpd) · [API integration guide](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/API_INTEGRATION_GUIDE.md)

**Review scope:** documentary findings dated **25 October 2026**, not an exhaustive production audit. This repository is not a contractual privacy policy, DPA or certification.

---

**Data controller — settled 13 November 2026.** **NeoMundi Recherche**,
association governed by the French law of 1 July 1901, RNA `W563015824`,
29 rue Tronchet, 75008 Paris —
[published identity](https://neomundi.org/mentions-legales). It carries
responsibility for NeoMundi's processing, **within the limits of the
processing actually carried out**.

🔴 **That settles who answers, not whether the processing is compliant.** It
also does not make this repository a privacy policy — it still is not one.

**→ [`docs/privacy/`](docs/privacy/README.md)** — technical documentation
**V0.1**, bilingual: categories and flows, erasure and retention,
recipients, and the limitations that remain open.

---

## What travels where
| Path | What the reviewed implementation shows | Boundary |
|---|---|---|
| Python SDK → measurement API | The client includes `llm_prompt` and `llm_response` when the caller supplies them. These fields are optional in the reviewed client. | Omitting text is not evidence that all semantic measurements are computed locally or remain available. |
| MCP → measurement API | In observation mode the adapter forwards the supplied execution object. In generation mode it sends generation arguments and the configured provider credential upstream. | MCP transport and downstream measurement transport are separate. The connector does not compute the measurements itself. |
| Measurement service → evaluators | Submitted text can be used for semantic analysis according to the route, configuration and enabled checks. | Identify the actual evaluator destinations and applicable provider terms for the deployment. Do not infer them from the name OBS or GOV. |
| Station records | Station's public documentation describes recording responses and measurement context in its internal ledger. | This is distinct from what its read-only interface returns. |
| Station read-only interface | The public contract describes fingerprints and lengths rather than raw prompts and responses. | A restricted read interface does not establish that raw text was never processed or stored upstream. |
| Interoperability export | The exchange contract defines its own fields and disclosure boundary. | An export without raw text does not prove that every internal flow is text-free. |

## Transmission and retention are different
There is no organization-wide claim here that no content is transmitted or retained.
Retention must be specified for each store, service, log, backup and evaluator
under the actual deployment configuration.

The reviewed local API source contains controls intended to exclude raw prompt
and response text from a measurement-log path. That is a scoped implementation
observation, not a full production storage audit or a guarantee covering Station,
provider services, other routes, backups or every log.

No universal retention duration or deployment-wide deletion guarantee has been
established by this documentation review.

## Minimize what you submit
Send only what the selected measurement needs. Do not include credentials in
prompt text or execution content. Use the documented authentication channels.
A future metrics-only client mode must identify where each metric is computed,
which measurements remain available, and how its provenance is retained.

Client-side provider generation does not, by itself, mean that its response
never leaves the client: a later measurement request may include that text.

## Review before integration
For the chosen deployment, establish the payloads, destinations, enabled
evaluators, storage locations, retention/deletion rules, access controls and
contractual responsibilities. Confirm the separation of client data and
authorized access before sharing pilot records.

This page documents technical boundaries; it is not a DPA, legal contract or certification.

## Sources and status
Documentary review dated 25 October 2026; no new production traffic was captured
for this page and evaluator retention policies were not independently audited.

- [SDK request construction](https://github.com/neomundi-io/neomundi-python-sdk/blob/main/src/neomundi/client.py)
- [MCP adapter](https://github.com/neomundi-io/neomundi-measurement-mcp/blob/main/runtime/adapter.mjs)
- [Measurement API documentation](https://github.com/neomundi-io/neomundi-runtime-measurement)
- [Station's published boundaries](https://github.com/neomundi-io/neomundi-station-public)
- [Exchange contract](https://github.com/neomundi-io/neomundi-measurement-interoperability)

Earlier descriptions remain in Git history and must not be treated as a current blanket confidentiality commitment.
