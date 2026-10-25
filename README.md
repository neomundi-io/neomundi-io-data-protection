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

## Sources and status
Documentary review dated 25 October 2026; no new production traffic was captured
for this page and evaluator retention policies were not independently audited.

- [SDK request construction](https://github.com/neomundi-io/neomundi-python-sdk/blob/main/src/neomundi/client.py)
- [MCP adapter](https://github.com/neomundi-io/neomundi-measurement-mcp/blob/main/runtime/adapter.mjs)
- [Measurement API documentation](https://github.com/neomundi-io/neomundi-runtime-measurement)
- [Station's published boundaries](https://github.com/neomundi-io/neomundi-station-public)
- [Exchange contract](https://github.com/neomundi-io/neomundi-measurement-interoperability)

Earlier descriptions remain in Git history and must not be treated as a current blanket confidentiality commitment.
