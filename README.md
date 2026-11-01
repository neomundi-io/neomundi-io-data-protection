# Data and confidentiality
[English](README.md) · [Français](README_FR.md)

NeoMundi has several data flows. A statement about an export does not describe
everything processed by the measurement API or recorded by Station.

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

## Additional findings — 1 November 2026

These clarifications reflect MCP deployment and measurement API audits reported during plugin qualification. They describe **the examined deployment**, not universal guarantees for every NeoMundi installation.

### Hosted MCP connector

- The `mcp.neomundi.org` service was reported deployed as connector version `0.3.0-rc.1` on an Infomaniak VPS, using HTTPS and MCP Streamable HTTP. This connector version is **not** the measurement version returned by the API (including `3.3.1` in the examined observation).
- For measurement requests, the connector forwards the necessary data to the measurement API. Submitted content can include prompts, responses and other execution material. The connector returns the resulting envelope; it does not compute the scores itself.
- Results may include measured signals, unpublished fields, textual elements such as `suspect_phrases`, measurement state, technical verdict and provenance. **Unpublished does not necessarily mean unmeasured**; equally, missing measurements must not be converted into favorable defaults.
- `ALLOW`, `FLAG` and `BLOCK` verdicts, when present, are not certifications, proofs of truth or general permissions to execute an action. Comparability depends on precisely documented versions and changes.

### Logging and retention: different boundaries

The reported audits provide the following **scoped observations**, not retention promises:

| Examined scope | Reported observation | Limitation |
|---|---|---|
| MCP connector | No application-side retention of prompts or responses identified in the audited connector | Says nothing about the API, client or AI provider |
| API nginx access logs | IP and User-Agent recorded; **14-day** retention reported | Depends on the deployed configuration |
| API container / journald logs | No rotation configured in the examined scope | No maximum retention established |
| Sample of API logs | No prompt or response found in **5,000 examined lines** | Negative sampling does not prove absence from all storage |
| Measurement database and backups | Retention rules not fully established | No automatic deletion interval should be assumed |

Technical logs, metrics, analyzed text and provenance evidence are **distinct categories**. The 14-day period above does **not** apply to all scientific observations or pilot accounts.

### Evaluators and providers

In the audited measurement deployment, three of the five examined signals used local evaluators, while a factuality path used an Infomaniak inference service. Actual destinations must not be inferred from a field name, presence of a key or OpenAI-compatible API format. A field containing `cohere` alone does not prove that Cohere was called.

The examined hosting was in Switzerland with Infomaniak. Provider retention, subprocessors and contractual terms still require confirmation for the **relevant service and contracting account**. A request to correct the Infomaniak contracting entity's details was pending as of 1 November 2026.

### Roles and public documents

NeoMundi Recherche, a French nonprofit association, sponsors the connector's pilot phase. Personal-data roles must be assessed **per processing activity and purpose**: a pilot may determine the purposes of some processing, while NeoMundi determines its own processing purposes, depending on the facts. An NDA or terms of use do not displace GDPR information obligations.

- [Legal notice](https://neomundi.org/mentions-legales)
- [Website privacy policy](https://neomundi.org/politique-de-confidentialite) — explicit MCP coverage is being prepared
- [French terms](https://neomundi.org/cgu) and [English terms](https://neomundi.org/en/terms-of-use) — confirm publication status and applicable version before relying on them

Pilot agreements, DPAs, operating licences and association records are **not** published in this repository. This technical explanation is not legal advice, a compliance certification or a guarantee that no personal data are processed.

## Sources and status
Documentary review dated 25 October 2026; no new production traffic was captured
for this page and evaluator retention policies were not independently audited.

- [SDK request construction](https://github.com/neomundi-io/neomundi-python-sdk/blob/main/src/neomundi/client.py)
- [MCP adapter](https://github.com/neomundi-io/neomundi-measurement-mcp/blob/main/runtime/adapter.mjs)
- [Measurement API documentation](https://github.com/neomundi-io/neomundi-runtime-measurement)
- [Station's published boundaries](https://github.com/neomundi-io/neomundi-station-public)
- [Exchange contract](https://github.com/neomundi-io/neomundi-measurement-interoperability)

Earlier descriptions remain in Git history and must not be treated as a current blanket confidentiality commitment.
