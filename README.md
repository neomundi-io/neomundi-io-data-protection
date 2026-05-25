# NeoMundi Data Protection

**Languages:** [English](./README.md) | [Français](./README_FR.md)

# NeoMundi Data Protection

Privacy architecture, data minimization and processing modes for the NeoMundi runtime measurement instrument.

NeoMundi is designed to measure generative AI systems at runtime while minimizing exposure to client content.

This repository documents the data protection architecture behind the NeoMundi instrument, including:

- OBS mode: post-generation observability without semantic content transmission.
- GOV mode: runtime governance with transient stream processing.
- BYOK architecture: the client keeps control of its LLM provider key.
- No content storage: prompts, responses and generated content are not retained by NeoMundi.
- Minimal measurement artifacts: NeoMundi produces technical signals, not content databases.
- Separation of responsibilities between client content, runtime measurement and governance decisions.

This repository is intended to support technical review, security assessment, legal review and DPA preparation.

It is not a legal contract.

---

## 1. Purpose of this repository

The purpose of this repository is to provide a clear, inspectable and versioned description of NeoMundi’s data protection architecture.

NeoMundi provides a runtime measurement instrument for generative AI systems.

The instrument produces governance-relevant signals such as:

- runtime stability;
- drift;
- validity signals;
- hallucination risk;
- semantic coherence;
- informational density;
- governance status;
- measurement artifacts.

NeoMundi does not seek to own, store or exploit client content.

The central principle is:

> Measure the behavior of AI systems without retaining their content.

---

## 2. Core privacy principle

NeoMundi is built around a strict separation between three layers:

1. Client application content  
   Prompts, responses, user data, business data, documents, instructions and generated outputs remain under the responsibility and control of the client.

2. Runtime measurement processing  
   NeoMundi may process technical or semantic signals depending on the selected mode, solely for the purpose of producing measurement signals.

3. Measurement artifacts  
   NeoMundi produces minimal artifacts such as request identifiers, timestamps, stability measurements, drift signals and governance status.

This separation is designed to support:

- data minimization;
- purpose limitation;
- privacy by design;
- privacy by default;
- auditability;
- client-controlled governance.

---

## 3. Operational modes

NeoMundi distinguishes two operational modes:

- OBS: Observability mode.
- GOV: Governance mode.

These modes differ in terms of when the instrument is called, what is transmitted, and what kind of processing occurs.

---

## 4. OBS mode: observability after generation

OBS mode is designed for post-generation observability.

In OBS mode, NeoMundi is not placed inside the live generation stream.

The client system generates the response using its own LLM stack, infrastructure and provider configuration.

NeoMundi receives only the metrics, technical traces or observation artifacts required to produce measurement signals.

### Key properties of OBS mode

In OBS mode:

- NeoMundi operates after generation.
- Semantic content is not transmitted to NeoMundi.
- Prompts are not transmitted.
- Responses are not transmitted.
- Generated content is not transmitted.
- The client keeps full control over its LLM provider key.
- NeoMundi produces observability signals from the provided artifacts.
- The client retains decision authority.

OBS mode is suitable for:

- post-generation monitoring;
- statistical analysis;
- model comparison;
- benchmarking;
- internal observability;
- research protocols;
- non-enforcing measurement workflows.

### OBS mode summary

    Client generates content
            ↓
    Client extracts or provides observation artifacts
            ↓
    NeoMundi measures
            ↓
    NeoMundi returns signals
            ↓
    Client interprets and governs

---

## 5. GOV mode: runtime governance during generation

GOV mode is designed for runtime governance.

In GOV mode, NeoMundi is placed inside the generation flow in order to produce measurement signals during execution.

In this mode, semantic content transits through NeoMundi during generation.

This processing is strictly transient and limited to runtime measurement.

NeoMundi does not retain prompts, responses or generated content.

### Key properties of GOV mode

In GOV mode:

- NeoMundi operates during generation.
- Semantic content transits through NeoMundi for runtime measurement.
- Processing is performed as transient stream processing.
- Prompts are not retained.
- Responses are not retained.
- Generated content is not retained.
- Content is not indexed.
- Content is not reused.
- Content is not used for model training.
- The client keeps control of its LLM provider key through BYOK.
- NeoMundi produces runtime governance signals.
- The client, client system, configured policy or responsible operator retains decision authority.

### GOV mode summary

    Client calls NeoMundi
            ↓
    NeoMundi processes the generation stream transiently
            ↓
    NeoMundi measures stability, drift and risk signals
            ↓
    NeoMundi returns governance artifacts
            ↓
    Client system applies its own policy

---

## 6. Transient stream processing

In GOV mode, semantic content is processed in stream during generation.

This means that NeoMundi may inspect the generation flow for the limited purpose of producing runtime measurement signals.

This processing is:

- temporary;
- purpose-limited;
- non-persistent;
- not used for training;
- not used to build a content database;
- not reused for commercial profiling;
- not retained after the measurement operation.

The purpose of transient stream processing is to measure the behavior of the AI system while it is generating.

Transit does not mean storage.

---

## 7. BYOK architecture

NeoMundi follows a BYOK architecture.

BYOK means: Bring Your Own Key.

The client uses its own LLM provider key.

NeoMundi does not provide the client’s underlying LLM inference key.

The client remains responsible for:

- its LLM provider relationship;
- its model choice;
- its provider configuration;
- its provider-side data processing terms;
- its own user data;
- its own application content;
- its own governance policies.

NeoMundi acts as a runtime measurement layer above the client’s AI stack.


---

## 7.1 Third-party measurement components

NeoMundi may use third-party technical services to produce certain measurement signals.

The client’s primary LLM generation remains under the client’s control through the BYOK architecture.

However, some NeoMundi measurement components may rely on external providers for specific scoring or evaluation tasks.

At the current stage, NeoMundi may use an OpenAI model as an internal LLM judge for selected measurement signals, such as hallucination risk, semantic validity or coherence evaluation, depending on configuration.

This use is limited to measurement purposes.

NeoMundi does not use third-party measurement components to store client content, build content datasets, train models on client content or create unrelated analytics.

Where a third-party measurement component receives semantic content, such transmission is limited to the purpose of producing the configured measurement signal.

This repository should therefore be read together with the applicable terms, data processing addenda and security documentation of the relevant third-party provider.

Future versions of NeoMundi may support alternative or configurable judge providers, including self-hosted, EU-hosted or client-selected evaluation components.

The purpose of this section is transparency: NeoMundi’s privacy architecture must document not only what NeoMundi stores, but also which technical components may participate in producing measurement signals.
---

## 8. No content storage

NeoMundi does not retain client content.

In both OBS and GOV modes, NeoMundi does not store:

- prompts;
- responses;
- generated outputs;
- uploaded documents;
- conversations;
- user messages;
- business content;
- semantic content databases.

NeoMundi does not reuse client content for:

- model training;
- fine-tuning;
- commercial enrichment;
- content indexing;
- dataset creation;
- unrelated analytics.

The purpose of NeoMundi is to produce measurement signals and governance artifacts, not to accumulate client content.

---

## 9. Minimal measurement artifacts

NeoMundi produces minimal artifacts designed for observability, traceability and governance.

Depending on configuration, artifacts may include:

- request ID;
- timestamp;
- stability score;
- drift signal;
- governance status;
- risk signal;
- validity signal;
- hallucination risk indicator;
- coherence indicator;
- informational density metric;
- regime classification;
- decision status such as ALLOW, FLAG or OBSERVE;
- technical metadata necessary for traceability.

These artifacts are not intended to reconstruct the original content.

The client remains responsible for any correlation between:

- internal request IDs;
- end users;
- generated content;
- application logs;
- governance decisions;
- business context.

---

## 10. Decision authority

NeoMundi provides signals.

NeoMundi does not replace the client’s decision authority.

The client system, configured policy or responsible operator remains responsible for:

- interpreting the signals;
- configuring thresholds;
- deciding when to observe;
- deciding when to alert;
- deciding when to slow down;
- deciding when to block;
- deciding when to escalate to human review;
- documenting its own governance policy.

NeoMundi’s position is:

> We provide the signal.  
> You keep decision authority.

---

## 11. Separation between signal and content

NeoMundi’s architecture separates content from signal.

The content belongs to the client.

The signal is produced by the measurement instrument.

This separation is central to the NeoMundi architecture.

It allows client teams to use NeoMundi for:

- AI observability;
- runtime governance;
- internal audit;
- compliance preparation;
- risk monitoring;
- model comparison;
- agent supervision;
- orchestration monitoring;
- scientific evaluation.

Without requiring NeoMundi to retain the underlying content.

---

## 12. Privacy by design

NeoMundi is designed according to privacy by design principles.

This means that data protection is considered at the architecture level, not added as an afterthought.

The architecture is based on:

- separation of roles;
- limited processing purposes;
- no content retention;
- minimal artifacts;
- transient runtime processing where required;
- client-side control of correlation;
- client-controlled LLM provider key;
- configurable governance;
- documented operational modes.

---

## 13. Privacy by default

NeoMundi also follows a privacy by default approach.

By default, the system is designed to avoid unnecessary content retention.

The default design assumptions are:

- do not store prompts;
- do not store responses;
- do not store generated content;
- do not reuse content;
- do not train on client content;
- minimize metadata;
- expose only necessary measurement artifacts;
- let the client retain authority and context.

---

## 14. Data minimization

NeoMundi’s data protection approach is based on minimization.

Only the information required to produce the runtime measurement signal should be processed.

The system is designed to avoid unnecessary collection of:

- user identifiers;
- personal data;
- business documents;
- long-term content history;
- conversational memory;
- raw prompts;
- raw responses;
- client-side logs.

Where semantic processing is necessary in GOV mode, it is limited to transient stream processing.

---

## 15. Purpose limitation

NeoMundi processes data only for the purpose of producing runtime observability and governance signals.

The purpose is not:

- content hosting;
- content storage;
- user profiling;
- behavioral advertising;
- model training on client content;
- resale of client data;
- creation of third-party datasets;
- enrichment of unrelated services.

The purpose is measurement.

---

## 16. Client responsibilities

The client remains responsible for:

- the lawful basis of its own processing;
- the content it submits to its AI system;
- the relationship with its end users;
- its own privacy notice;
- its own LLM provider configuration;
- its own DPA with its LLM provider where applicable;
- its internal correlation between request IDs and content;
- its own retention policies;
- its own governance thresholds;
- its own operational decisions.

NeoMundi does not determine the business purpose of the client’s AI system.

NeoMundi provides a measurement instrument.

---

## 17. NeoMundi responsibilities

NeoMundi is responsible for documenting and operating the measurement layer according to its stated architecture.

This includes:

- distinguishing OBS and GOV modes;
- limiting processing to measurement purposes;
- avoiding retention of prompts and responses;
- maintaining clear technical documentation;
- supporting auditability of the instrument;
- producing minimal measurement artifacts;
- documenting privacy and governance assumptions;
- supporting legal review and DPA preparation.

---

## 18. Legal positioning

This repository is a technical and architectural description.

It is designed to support legal assessment.

It is not itself:

- a DPA;
- a privacy policy;
- a legal opinion;
- a contractual commitment;
- a compliance certification.

A formal DPA, privacy notice or contractual clause should be prepared or reviewed by qualified legal counsel.

The purpose of this repository is to make the technical architecture clear enough for legal professionals, DPOs, security teams and client organizations to assess the system accurately.

---

## 19. DPA preparation brief

A DPA or contractual addendum should reflect the following architecture:

NeoMundi provides two documented modes of operation.

### Third-party measurement providers

Where NeoMundi uses third-party providers to produce specific measurement signals, such as an LLM judge, the DPA or contractual documentation should identify:

- the provider used;
- the type of signal produced;
- whether semantic content is transmitted;
- the purpose of the transmission;
- the applicable retention and training terms;
- the relevant subprocessors or data processing terms;
- whether an alternative configuration is available.

At the current stage, OpenAI may be used as an internal LLM judge for selected measurement signals, depending on configuration.

### OBS mode

In OBS mode:

- NeoMundi operates after generation.
- Semantic content is not transmitted to NeoMundi.
- Prompts and responses are not transmitted.
- Only metrics or observation artifacts are processed.

### GOV mode

In GOV mode:

- NeoMundi operates during generation.
- Semantic content transits through NeoMundi.
- Processing is strictly transient.
- Processing is performed only to produce runtime measurement signals.
- Prompts and responses are not retained.
- Generated content is not retained.
- Content is not reused for training.

### In both modes

- The client uses its own LLM provider key.
- NeoMundi does not retain content.
- NeoMundi produces minimal artifacts.
- The client remains responsible for content, correlation and final decision-making.
- NeoMundi provides signals, not final legal or operational decisions.

---

## 20. Suggested DPA wording for legal review

The following wording may be used as a starting point for legal review.

It should not be used as a final legal clause without professional validation.

> NeoMundi provides a runtime measurement instrument for generative AI systems. The service distinguishes two operational modes.
>
> In OBS mode, the service operates after generation and does not receive prompts, responses or semantic content. Only metrics, technical identifiers or observation artifacts necessary to produce measurement signals are processed.
>
> In GOV mode, the service operates during generation. Semantic content transits through NeoMundi solely for the purpose of transient stream processing and runtime measurement. Prompts, responses and generated content are not logged, retained, indexed, reused or used for model training.
>
> In both modes, the client remains responsible for its own LLM provider relationship and uses its own provider key under a BYOK architecture. NeoMundi produces minimal measurement artifacts, such as request identifiers, timestamps, stability measurements, drift indicators and governance signals.
>
> The client remains responsible for the correlation between request identifiers, users, content, business context and operational decisions. NeoMundi provides measurement signals and does not replace the client’s decision authority.

---

## 21. Security and auditability considerations

NeoMundi’s data protection architecture is designed to support auditability.

Relevant audit dimensions include:

- mode used: OBS or GOV;
- request identifier;
- timestamp;
- measurement signal;
- governance status;
- threshold configuration;
- artifact structure;
- absence of retained prompts;
- absence of retained responses;
- separation between content and signal.

Future versions of this repository may include:

- technical diagrams;
- data flow maps;
- retention tables;
- security questionnaires;
- subprocessors;
- deployment assumptions;
- sample DPA annexes;
- audit checklist;
- risk assessment templates.

---

## 22. Data flow overview

### OBS mode

    Client LLM stack
            ↓
    Generation completed
            ↓
    Client-side observation artifacts
            ↓
    NeoMundi measurement
            ↓
    Minimal signal artifact
            ↓
    Client governance layer

### GOV mode

    Client application
            ↓
    NeoMundi runtime measurement layer
            ↓
    Client LLM provider through BYOK
            ↓
    Streaming generation
            ↓
    Transient measurement
            ↓
    Minimal governance artifact
            ↓
    Client policy / operator / system decision

---

## 23. What NeoMundi is

NeoMundi is:

- a runtime measurement instrument;
- a signal layer;
- an observability layer;
- a governance support layer;
- an auditability component;
- a privacy-minimized measurement system.

---

## 24. What NeoMundi is not

NeoMundi is not:

- a content storage system;
- a prompt database;
- a response database;
- a model training platform using client content;
- a replacement for the client’s DPO;
- a replacement for the client’s legal obligations;
- a final decision-maker;
- a universal guarantee of factual truth;
- a substitute for human oversight where required.

---

## 25. Relation to AI governance

NeoMundi’s position is that AI governance requires runtime measurement.

Static policies are necessary, but they are not sufficient.

A generative AI system can behave differently depending on:

- prompt context;
- model version;
- provider behavior;
- orchestration chain;
- agentic loops;
- retrieval context;
- tool calls;
- runtime instability;
- semantic drift.

NeoMundi provides signals that help teams observe, document and govern these behaviors.

---

## 26. Relation to compliance

NeoMundi can support compliance workflows by producing runtime measurement artifacts.

These artifacts may help client organizations document:

- observability;
- traceability;
- runtime monitoring;
- governance thresholds;
- escalation policies;
- audit trails;
- operational supervision.

However, NeoMundi does not itself certify compliance.

Compliance depends on the client’s full system, legal basis, operational policy, contractual framework and organizational controls.

---

## 27. Recommended legal review questions

When preparing a DPA or legal assessment, the following questions should be reviewed:

1. In which mode is NeoMundi used: OBS, GOV, or both?
2. What data is transmitted in each mode?
3. Does GOV mode process personal data in the client’s specific use case?
4. What is the lawful basis for the client’s use case?
5. What LLM provider is used by the client?
6. What are the LLM provider’s own data processing terms?
7. What measurement artifacts are retained by the client?
8. What correlation does the client maintain between request IDs and content?
9. What retention period applies to measurement artifacts?
10. What governance decisions are automated, assisted or human-reviewed?
11. What thresholds are configured by the client?
12. What audit trail is required by the client’s sector?
13. What security requirements apply to the deployment?
14. What contractual clauses are required between the parties?

---

## 28. Recommended terminology

For consistency, the following terminology should be used:

- runtime measurement instrument;
- transient stream processing;
- minimal measurement artifact;
- no content retention;
- BYOK architecture;
- client-controlled decision authority;
- OBS mode;
- GOV mode;
- data minimization;
- privacy by design;
- privacy by default;
- separation between content, signal and decision.

Avoid ambiguous wording such as:

- “NeoMundi never processes content”;
- “NeoMundi is fully compliant by default”;
- “NeoMundi guarantees legal compliance”;
- “NeoMundi guarantees factual truth”;
- “NeoMundi replaces human oversight”.

Preferred wording:

> In GOV mode, semantic content transits through NeoMundi solely for transient runtime measurement and is not retained.

---

## 29. Public summary

NeoMundi measures AI systems without retaining their content.

In OBS mode, semantic content is not transmitted.

In GOV mode, semantic content transits temporarily for runtime measurement and is not stored.

The client keeps its own LLM provider key.

NeoMundi returns minimal measurement artifacts.

The client keeps decision authority.

---

## 30. Status

This repository is part of the public NeoMundi documentation.

It is intended to evolve as the instrument, legal framework, security documentation and governance model mature.

Current status:

Draft for technical and legal review.  
Not a legal contract.  
Not a final DPA.

---

## 31. Links

NeoMundi GitHub organization:  
https://github.com/neomundi-io

NeoMundi LLM Cartography:  
https://github.com/neomundi-io/llm-cartography

NeoMundi website:  
https://neomundi.io

NeoMundi Research website:  
https://neomundi.org
