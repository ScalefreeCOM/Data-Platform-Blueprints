# Cloud Healthcare Data Platform Blueprint

**Create a governed longitudinal view across providers without losing clinical context, provenance, or purpose restrictions.**

Audience: provider-network leadership, clinical informatics, care coordination, privacy, research governance, and data teams. Updated: 5 November 2026.

## Platform recommendation and decision assumptions

**Recommended starting point: Snowflake on Azure.** This recommendation assumes a provider network with Azure-centered integration, SQL reporting skills, and multiple independently governed participating organizations. It is an architectural judgment to validate, not a claim that an industry has one universally best platform.

Structured referral, encounter, and identity-crosswalk data needs explicit sharing boundaries and reproducible cross-provider products.

Use controlled interface-engine/API outputs, provider-scoped datasets and roles, a validated Vault target, and purpose-specific marts.

**Strongest alternatives:** Fabric for a predominantly Microsoft BI operational network; Databricks where imaging or large clinical NLP/research engineering is central.

| Candidate | Fit and conditions to test |
| --- | --- |
| [Microsoft Fabric on Azure](https://learn.microsoft.com/en-sg/fabric/fundamentals/decision-guide-lakehouse-warehouse) | Alternative to test. A candidate for SQL/Power BI delivery with a tested Warehouse target. Validate capacity contention, licenses, semantic-model behavior, and actual source connectivity. |
| [Snowflake on Azure](https://docs.snowflake.com/en/user-guide/cost-understanding-overall) | Recommended starting point under the stated assumptions. A candidate for Azure-centered SQL integration and independently attributed transformation/serving workloads. Co-locate sources where practical and cost external data movement. |
| [Snowflake on AWS](https://docs.snowflake.com/en/user-guide/cost-understanding-overall) | Alternative to test. A candidate for AWS-centered SQL integration and independently attributed transformation/serving workloads. Verify region, provider integrations, and contract pricing. |
| [Google Cloud with BigQuery](https://cloud.google.com/bigquery/pricing) | Alternative to test. A candidate for managed SQL with on-demand or capacity-based economics. Test scan patterns, concurrency, project controls, and data-location alignment. |
| [Databricks on Azure, AWS, or Google Cloud](https://docs.databricks.com/aws/en/getting-started/architecture) | Alternative to test. A candidate for mixed event/file engineering, SQL, and model workflows. Choose the cloud near the data and skills; validate actual feature/compute availability. |

### Industry benchmark and cost boundary

Reconstruct a referral cohort, apply a patient unmerge, and revoke one participant’s access while testing concurrent provider reporting. Use identical source scope, correction cases, service targets, and business definitions when comparing platforms.

Report cost per **accepted referral/encounter batch and governed network product refresh**, plus end-to-end freshness, concurrent-consumer response, reconciliation exceptions, and operator hours. Specifically measure duplicate provider extracts, identity-reprocessing work, permission administration, and separate research copies.

Evaluate governed sharing and lineage against actual provider agreements; new AI services require clinical/purpose validation and separate cost attribution.


## Follow a referral across organizations

A healthcare network needs to understand whether a referral reached the receiving service, whether an appointment occurred, and whether the result returned to the originating clinician. The integration problem spans identity, coding, timestamps, and organizational authority. This blueprint focuses on cross-provider coordination and analytical reuse. The separate hospital blueprint focuses on operations inside a hospital.

The first product is referral-to-completed-encounter reporting across two participating providers. Start with structured referral, scheduling, encounter, and organization data. Clinical decision support, prescribing, and automated treatment recommendations require separate clinical validation and operational designs.

| Stage | Event and evidence | Decision supported |
| --- | --- | --- |
| Referral issued | Referral/order ID, reason code, originating organization, issue time | Establish the requested service and accountable provider. |
| Referral received | Receiving ID, triage status, acceptance time | Identify rejected, redirected, or unacknowledged requests. |
| Appointment arranged | Appointment status, rescheduling chain, service location | Measure access while retaining cancellations. |
| Encounter completed | Encounter ID, completion status, actual timing | Distinguish delivered care from a scheduled slot. |
| Result returned | Result/report metadata and delivery status | Identify coordination gaps under authorized access. |

## Identity and clinical meaning form the integration contract

Keep patient identifiers namespaced by issuer and organization. An enterprise patient index or approved matching service supplies governed cross-provider matches; do not join patients solely by name, date of birth, or a generated hash. Preserve match provenance, confidence/status, merges, and unmerges.

Separate patient, practitioner, organization, referral, appointment, encounter, observation, specimen, and result/report. Version terminology mappings and preserve original codes, units, clinical status, and correction status. A preliminary result and an amended final result represent different source states.

Use [HL7 FHIR](https://hl7.org/fhir/overview.html) interfaces where the actual systems support them, with an explicit FHIR version and implementation profile. FHIR resources are an exchange representation; flattening them into tables must preserve repeated values, references, provenance, and history needed by the use case. dlt requires validated resources/adapters and pagination/history behavior; it is not assumed to be a clinical interface engine.

## A referral metric with an explicit denominator

Suppose a cohort contains 100 accepted referrals. By the agreed cutoff, 65 have a completed qualifying encounter, 10 are cancelled, and 25 remain open. A defined completion proportion for that original accepted cohort is 65%. A denominator excluding cancellations would yield 65/90, or approximately 72.2%; publish neither without declaring the rule.

The fact grain is **referral × destination service × status-effective event**, with appointment and encounter links. Record transfers to another provider rather than counting a new referral automatically. Include cohort entry date, reporting cutoff, and the policy for withdrawn or duplicate referrals.

Users add Business Vault entities for approved patient crosswalks, service taxonomies, referral episodes, and elapsed-time measures. Flow.BI output proceeds through FCT and TurboVault; the review occurs after model generation and before datavault4dbt/dbt execution.

## Protect care relationships and secondary uses

Assign access by participating organization, clinical purpose, and approved data product. Separate direct-care, operational planning, and research permissions. Pseudonymization does not by itself authorize cross-provider sharing or make records anonymous. Protect small cohorts and rare-condition combinations in externally shared aggregates.

For EU settings, map [GDPR](https://eur-lex.europa.eu/eli/reg/2016/679/oj/eng) requirements and applicable health-sector law. For relevant US operations, assess [HIPAA covered-entity/business-associate status](https://www.hhs.gov/hipaa/for-professionals/covered-entities/index.html) and necessary arrangements. Do not assume these regimes apply identically to every participant.

Flow.BI's local Docker profiler must read authorized data within the selected Azure, AWS, or Google Cloud boundary. Classify its outbound derived metadata and statistical fingerprints before approving the backend connection. Patient or condition names in structural metadata can also be sensitive.

## Demonstrate coordination before scaling

Clinical informatics approves interpretation, participating providers approve sharing, identity stewards manage match exceptions, and privacy/research owners govern secondary use. Train analysts on encounter/referral definitions and operators on identity corrections and data-access auditing.

The pilot must account for every source referral, preserve rescheduling history, handle patient unmerge, distinguish cancelled from completed encounters, and reproduce cohort results at two cutoffs. Test revoked participant access and suppression of small exported groups. Acceptance includes referral/encounter reconciliation and source-to-report lineage reviewed by clinical owners.

Expand into longitudinal quality measures and research-ready products only after these foundations work. The complete technical, deployment, training, and reference guidance follows within this document.

## Implement the analytical pipeline

The common automation chain is **Flow.BI → FCT → TurboVault → review generated dbt models → datavault4dbt/dbt execution**. Users then add **Business Vault entities** for the industry rules described above. Keep generated Raw Vault artifacts and user-authored extensions in separate project paths.

Flow.BI requires actual source data. Source metadata is mostly optional and improves results. Its [local-processing Docker image](https://flow.bi/blog/how-flow-bi-protects-sensitive-enterprise-data/) runs in the customer's Azure, AWS, or Google Cloud environment and sends derived metadata/statistical fingerprints to the Flow.BI backend. Approve source access and the outbound payload as part of the processing design.

Use [dlt](https://github.com/dlt-hub/dlt) for supported API, database, and file ingestion. Select source-specific cursor/CDC or snapshot behavior, persistent pipeline state, restart rules, deletion detection, and schema-change handling. Specialized clinical, industrial, carrier, or streaming acquisition requires an authorized, compatible interface or gateway. A timestamp cursor alone does not guarantee capture of every intermediate event or delete.

[FCT](https://www.scalefree.com/consulting/turbovault/flowbi-connector/) connects the Flow.BI model to the generator workflow. [TurboVault4dbt](https://github.com/ScalefreeCOM/turbovault4dbt) and [TurboVault Engine](https://github.com/ScalefreeCOM/turbovault-engine) are distinct generator editions; select the supported integration route. Request the online TurboVault Studio through Scalefree as described under Next steps. Keep the selected generator's metadata contract in the release record.

[datavault4dbt](https://github.com/ScalefreeCOM/datavault4dbt) supplies staging and Vault macros; [dbt](https://github.com/dbt-labs/dbt) compiles/tests/runs models against the target adapter. Pin runtime, adapter, package, generator, and target versions. Validate decimal/time semantics, hashing, repeated loads, corrections, required writes, and performance together. Cloud hosting does not remove these integration contracts.

Use managed target tables for the first pilot unless an open-table design has a justified requirement. If adding Iceberg/external tables, assign catalog, writer, retention, maintenance, permission, and recovery owners. Do not place two refresh engines in charge of the same object without a deliberate design.

## Operate and govern by data product

Assign each product a business owner, technical operator, purpose, grain, definitions, refresh target, classification, retention, and quality thresholds. Preserve source batch/offset IDs, code/package versions, calculation rules, cutoff, and exception status with published outputs. Keep operational-system decisions and regulated calculation engines within their validated authority boundaries.

Use institutional identity, scoped service principals, separate development/production, secret management, auditable privileged access, and approved network routes. Enforce restrictions through SQL, BI, export, and underlying storage. Test actual negative access and revocation behavior. A managed service or compliant provider does not establish application-level compliance.

Choose orchestration and alerts around the product's dependencies. A late source or unresolved material reconciliation break should affect publication status. Define retries and recovery of ingestion state, metadata/catalogs, data, credentials, and deployment artifacts. Demonstrate rollback and a clean-environment restore.

## Compare lifecycle cost and delivery value

The managed-cloud case is **shorter delivery time, reduced infrastructure administration, elastic execution, and useful platform integration**. The decision is an operating-model tradeoff. Keep privacy, contractual requirements, data-location constraints, and an exit plan even when sovereignty is not the main objective.

Use a three-year model with an explicit growth scenario:

`TCO = migration and implementation + platform/compute + storage and retained copies + ingestion/orchestration + network/egress + BI/security licenses + support + operating staff + recovery/testing + exit allowance`

Account for negotiated rates and existing commitments as incremental costs, without treating sunk licenses or staff effort as free. Measure normal, peak, backfill, and quiet-period workloads. Record costs per completed data-product refresh and per meaningful business unit, together with correctness and service quality. Compare low/base/high demand and sensitivity to refresh frequency, retention, concurrency, and source growth. Do not publish a cost-saving percentage without the corresponding workload and pricing assumptions.

For feature decisions, record the use case, production availability in the selected region/edition, integration limits, expected outcome, incremental cost, operating owner, and rollback. Prefer an ordinary SQL or scheduled pipeline when it meets the product goal more economically. New AI, streaming, or sharing capabilities should earn their place through a measured pilot.

## Delivery gates

1. Agree the product's grain, calculation, scope, cutoff, access, and reconciliation targets with its business owner.
2. Benchmark the recommended platform against the strongest alternative using the same sources and workload, including cold/warm runs and concurrent consumers.
3. Build the ingestion/model-generation path; review after TurboVault; run tested macros and add Business Vault rules.
4. Prove the industry's worked example and failure cases, historical reconstruction, permissions, restart, restore, and regeneration behavior.
5. Compare measured run cost and staff effort with the business case. Document limitations and choose the platform before committing to broad migration.

## Training and knowledge transfer

Make capability development a delivery workstream with practical acceptance criteria.

| Track | Training resource | Participants | Proposed project outcome |
| --- | --- | --- | --- |
| DV — Data Vault | [Data Vault Training & Certification](https://www.scalefree.com/data-vault-2-1-trainings/data-vault-training-certification/) and [Modeling Class](https://www.scalefree.com/data-vault-2-1-trainings/data-vault-2-1-modeling-class/) | Architects, modelers, engineering leads; selected stewards for shared concepts. | Understand generated Raw Vault structures and design additional Business Vault entities with the correct keys, grain, and history. |
| DWA — Data Warehouse Automation | [Scalefree dbt training: DataVault4dbt and TurboVault automation blocks](https://www.scalefree.com/training/dbt-training/) | Modelers, engineers, and release owners. | Operate the Flow.BI–FCT–TurboVault workflow, review generated models before datavault4dbt/dbt execution, and regenerate without overwriting user-authored Business Vault entities. |
| dbt | [Scalefree dbt Training](https://www.scalefree.com/training/dbt-training/) | Data engineers, analytics engineers, and operators. | Build and test models, use packages/macros, debug failures, and promote releases through controlled environments. |
| Flow.BI foundations | [Flow.BI Introduction](https://flow.bi/trainings/flow-bi-introduction/) | Business analysts, domain stewards, architects, project leads, and engineers. | Understand the interface, data sources, deployment targets, and first model-generation run. |
| Flow.BI configuration and operations | [Flow.BI In-Depth](https://flow.bi/trainings/flow-bi-in-depth/) | Engineers, architects, and platform operators. | Configure classification, permissions, API access, and integration settings for the organization environment. |
| Understanding generated models | [Generated Data Vault Models](https://flow.bi/trainings/generated-data-vault-2-0-models/) | Data modelers, engineers, and architects. | Interpret generated structures and their downstream physical implementation; apply this knowledge to the review after TurboVault and to Business Vault extensions. |
| Flow.BI metadata presentation | [Presenting Flow.BI Metadata — training catalog](https://flow.bi/trainings/) | Modelers, analysts, and metadata/catalog owners. | Use Flow.BI dashboards to present internal metadata to authorized users. |

**Terminology:** DWA means Data Warehouse Automation. The DWA training track uses Scalefree's published automation learning blocks. DVA refers exclusively to the Data Vault Alliance.

The Data Vault certification training covers Data Vault 2.1. Align the curriculum with the selected tool versions and implementation patterns. Its dbt offering is customizable; agree the agenda and prerequisites before booking.
Flow.BI In-Depth requires Flow.BI Introduction and general data-architecture knowledge. Generated Data Vault Models requires Flow.BI Introduction and Data Vault modeling knowledge. Use the [Flow.BI training catalog](https://flow.bi/trainings/) to select classes or a bundle and confirm current delivery details.

Recommended sequence:

1. Run DV foundations and modeling to understand the generated Raw Vault and design Business Vault extensions.
2. Complete Flow.BI Introduction before connecting the pilot sources; follow with In-Depth for the configuration and operations team and Generated Data Vault Models for modelers and engineers.
3. Run dbt fundamentals before building the first transformation pipeline.
4. Run DWA practical sessions when the first Flow.BI output is available for direct processing through FCT and TurboVault. Include metadata presentation training for the team publishing model documentation.
5. Complete a organization-led lab using synthetic datasets for the selected industry workflow: dlt ingestion, Flow.BI generation, direct FCT transfer, TurboVault generation, generated-model review, datavault4dbt/dbt execution, user-added Business Vault entities, and industry-specific reconciliation.
6. Require at least two internal operators to deploy, troubleshoot, restore, and explain the pipeline without external assistance.

Budget training, preparation time, labs, and follow-up coaching separately from software and hosting. Use the [current training catalog](https://www.scalefree.com/training/) for available formats and scheduling.

## Scalefree implementation resources

The following articles complement the product documentation and training. Read the Flow.BI/FCT integration overview first, then TurboVault generation, datavault4dbt staging and loading, and finally Business Vault extensions and deployment practices.

| Tool or topic | Scalefree blog resource | Application to this blueprint |
| --- | --- | --- |
| Flow.BI, FCT, TurboVault, and datavault4dbt | [Flow.BI: Generating the Raw Data Vault Using AI and dbt](https://www.scalefree.com/blog/modeling/flow-bi-generating-the-raw-data-vault-using-ai-and-dbt/) | Explains the logical-to-physical automation chain and connector, and the addition of business logic in the Business Vault. Pair it with the [FCT access page](https://www.scalefree.com/consulting/turbovault/flowbi-connector/) for onboarding. |
| TurboVault | [Speed Up Your Data Vault 2.0 Implementation with TurboVault4dbt](https://www.scalefree.com/blog/tools/speed-up-your-data-vault-2-0-implementation-with-turbovault4dbt/) | Introduces metadata-driven dbt model generation. Use it to understand the generation stage before the review phase. |
| datavault4dbt overview | [Kick-Start Your Data Vault 2.0 Implementation with datavault4dbt](https://www.scalefree.com/blog/tools/kick-start-your-data-vault-2-0-implementation-with-datavault4dbt/) | Introduces loading templates and the package's role within dbt. Useful background before the hands-on training. |
| datavault4dbt staging | [Exploring datavault4dbt — Vol. 1: The Staging Layer](https://www.scalefree.com/blog/tools/exploring-datavault4dbt-a-practical-series-on-the-dbt-package-for-data-vault-2-0-vol-1-the-staging-layer/) | Covers staging configuration, hashing, derived columns, and prejoins. Apply these concepts when connecting the dlt landing datasets to generated transformations. |
| datavault4dbt Raw Vault | [Exploring datavault4dbt — Vol. 2: Standard Entities in the Raw Vault](https://www.scalefree.com/blog/datenwerkzeuge/exploring-datavault4dbt-a-practical-series-on-the-dbt-package-for-data-vault-2-0-vol-2-standard-entities-in-the-raw-vault/) | Walks through hubs, links, and satellites. Use it to interpret the models produced by TurboVault and their macro parameters. |
| Business Vault extensions | [Designing the Business Vault: Key Strategies for Effective Data Organization](https://www.scalefree.com/blog/data-vault/designing-the-business-vault-key-strategies-for-effective-data-organization/) | Explains reusable business logic and additional entities such as computed satellites, links, PITs, and bridges. Relevant to user-defined organization extensions after Raw Vault generation. |
| dbt delivery and testing | [CI/CD: Practical Insights into Automating Data Vault 2.0 with dbt](https://www.scalefree.com/blog/data-vault/ci-cd-practical-insights-into-automating-data-vault-2-0-with-dbt/) | Uses datavault4dbt and GitHub Actions to illustrate testing and delivery automation. Adapt execution and runner placement to the organization's cloud boundary. |
| PostgreSQL workload assessment | [Architectural Limitations of PostgreSQL for Enterprise Data Vault Workloads](https://www.scalefree.com/blog/data-vault/architectural-limitations-of-postgresql-for-enterprise-data-vault-workloads/) | Discusses analytical scaling and join-heavy workload considerations. Use it to frame representative benchmarks when deciding between the relational pilot and lakehouse patterns. |
| Sovereignty, Iceberg, and Trino | [The Sovereign Data Platform: The Path to Data Ownership and Secure AI](https://www.scalefree.com/blog/architecture/the-sovereign-data-platform-the-path-to-data-ownership-and-secure-ai/) | Discusses controlled object storage, open table formats, and replaceable query engines, including Trino. Supports the portability and storage/compute separation principles. |

Examples in older articles may use earlier package versions, metadata layouts, or specific cloud databases. Use the pinned tool versions and current documentation for implementation details. For dlt ingestion and Trino/Iceberg configuration, use the primary documentation already linked in the platform and ingestion guidance in this document alongside these architectural resources. Browse the [Scalefree blog](https://www.scalefree.com/blog/) for further articles.

## Flow.BI implementation resources

Use the [Flow.BI blog](https://flow.bi/blog/) for ongoing product architecture and implementation material. Relevant starting points:

| Resource | Why it matters to this blueprint |
| --- | --- |
| [How Flow.BI Protects Sensitive Enterprise Data](https://flow.bi/blog/how-flow-bi-protects-sensitive-enterprise-data/) | Explains required data profiling, the local Docker image, storage access, and the local/backend processing boundary. |
| [Seeing Every Step: Data Lineage Inside Flow.BI](https://flow.bi/blog/seeing-every-step-data-lineage-inside-flow-bi/) | Describes source-to-model traceability and graph views. Use this model lineage alongside dlt load evidence and dbt execution lineage. |
| [Turning Salesforce and Pardot Tables Into a Semantic Model](https://flow.bi/blog/turning-salesforce-and-pardot-tables-into-a-semantic-model/) | Illustrates concept recognition and declared versus inferred relationships using CRM data. Use it as background on model generation; organization-specific additions in this blueprint are implemented through Business Vault entities. |
| [Generative AI for Data Vault](https://flow.bi/ai-driven-business/) | Explains the progression from data profiling to a logical Data Vault model and downstream physical-model automation. |
| [Flow.BI Media Library](https://flow.bi/media/) | Includes local-processing, bring-your-own-bucket, modeling, and dbt integration sessions for team onboarding. |

## Next steps

Select the pilot described in this blueprint, name its business and technical owners, and bring representative source data, reporting definitions, workload volumes, and operating constraints to an architecture discussion.

| Step | Where to go | What to request or prepare |
| --- | --- | --- |
| Discuss the implementation | [Scalefree contact form](https://www.scalefree.com/about-scalefree/contact/) | Request a platform assessment, pilot workshop, implementation support, or tailored training. Include the blueprint title and the first data product. |
| Try automated modeling | [Flow.BI trial registration](https://flow.bi/sign-up-for-trial/) | Request a trial and local-processing Docker deployment instructions. Prepare authorized source data; available metadata improves the result. |
| Obtain the connector | [FCT access and download request](https://www.scalefree.com/consulting/turbovault/flowbi-connector/) | Request Flow.BI Connector for TurboVault4dbt access and verify the intended generator version. |
| Install the macro package | [datavault4dbt repository and releases](https://github.com/ScalefreeCOM/datavault4dbt) | Obtain the package, choose a supported target adapter, and pin a tested version. |
| Access the online generator | [Request TurboVault Studio signup through Scalefree](https://www.scalefree.com/about-scalefree/contact/) | Ask specifically for the new online TurboVault Studio and its Flow.BI/FCT integration route. |
| Obtain a downloadable generator | [TurboVault4dbt](https://github.com/ScalefreeCOM/turbovault4dbt) and [TurboVault Engine](https://github.com/ScalefreeCOM/turbovault-engine) | Select the edition that fits the integration and retain its version and configuration. |
| Receive implementation updates | [Scalefree newsletter registration](https://www.scalefree.com/) | Use the “Data Vault 2.GO / Build Better Data Platforms” newsletter form on the homepage. |

Complete the pilot's reconciliation, access, recovery, and cost checks before extending the source scope. Record the results and the decision to proceed in the project repository.
