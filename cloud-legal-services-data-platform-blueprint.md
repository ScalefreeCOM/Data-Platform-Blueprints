# Cloud Legal Services Data Platform Blueprint

**Connect matters, people, work, billing, and knowledge while preserving confidentiality, ethical walls, and privilege-sensitive boundaries.**

Audience: managing partners, practice operations, conflicts teams, finance, knowledge management, records, and data teams. Updated: 5 November 2026.

## Legal Services data platform: tools and responsibilities

[dlt](https://github.com/dlt-hub/dlt) (data load tool) is the ingestion library for supported API, database, and file feeds. In this legal services design, it brings practice-management, time, and billing into controlled landing datasets with load evidence. [Flow.BI](https://flow.bi/) turns patterns in the supplied data into a logical integration model. The modeling input includes client, matter, and engagement; actual data is required, while available source metadata improves the result.

For the legal services model, [FCT](https://www.scalefree.com/consulting/turbovault/flowbi-connector/) (Flow.BI Connector for TurboVault4dbt) provides the model-transfer step between Flow.BI and TurboVault. [TurboVault](https://www.scalefree.com/consulting/turbovault/) produces the generated model files that the engineering team reviews. Review those generated artifacts before executing the legal services transformations; Studio is the online access option, requested through Scalefree.

[datavault4dbt](https://github.com/ScalefreeCOM/datavault4dbt) implements the Data Vault transformation patterns referenced by the generated project. It contributes the technical history-loading foundation for client, matter, and engagement. [dbt](https://github.com/dbt-labs/dbt) turns the versioned models and macros into an executable, testable database build. User-defined Business Vault models add effective fee rules and billing/collection measures within that workflow.

[Microsoft Fabric Warehouse](https://learn.microsoft.com/en-sg/fabric/fundamentals/decision-guide-lakehouse-warehouse) is the reference target for this legal services architecture. Microsoft Fabric is the managed analytics platform; its Warehouse is the SQL target for staging, Vault models, and marts in this design. Power BI supplies the reporting/semantic delivery path. Azure is the cloud ecosystem for the Fabric deployment and the customer-controlled source/profiling environment; tenant identity, networking, storage, capacity, and execution need explicit owners. Size and operate these services against the legal services pilot rather than a generic platform benchmark.

[Scalefree](https://www.scalefree.com/) is a data and AI consultancy providing strategy, platform implementation, Data Vault expertise, and training. For this blueprint, that support can include the legal services source-to-model design, implementation of effective fee rules and billing/collection measures, and training the team to maintain the resulting product.

## Platform recommendation and decision assumptions

**Recommended starting point: Microsoft Fabric on Azure.** This recommendation assumes a Microsoft-oriented law firm emphasizing structured matter/time/billing products and existing Power BI administration. It is an architectural judgment to validate, not a claim that an industry has one universally best platform.

Matter financial reporting can align with established identity and BI skills when ethical-wall access is enforced through every serving path.

Use approved practice-management extracts, a Warehouse target, and matter-restricted marts/semantic models. Keep content indexing a separate authorized scope.

**Strongest alternatives:** Snowflake on Azure for independently operated reporting workloads; Databricks for a separately governed document-processing program; BigQuery for an existing GCP estate.

| Candidate | Fit and conditions to test |
| --- | --- |
| [Microsoft Fabric on Azure](https://learn.microsoft.com/en-sg/fabric/fundamentals/decision-guide-lakehouse-warehouse) | Recommended starting point under the stated assumptions. A candidate for SQL/Power BI delivery with a tested Warehouse target. Validate capacity contention, licenses, semantic-model behavior, and actual source connectivity. |
| [Snowflake on Azure](https://docs.snowflake.com/en/user-guide/cost-understanding-overall) | Alternative to test. A candidate for Azure-centered SQL integration and independently attributed transformation/serving workloads. Co-locate sources where practical and cost external data movement. |
| [Snowflake on AWS](https://docs.snowflake.com/en/user-guide/cost-understanding-overall) | Alternative to test. A candidate for AWS-centered SQL integration and independently attributed transformation/serving workloads. Verify region, provider integrations, and contract pricing. |
| [Google Cloud with BigQuery](https://cloud.google.com/bigquery/pricing) | Alternative to test. A candidate for managed SQL with on-demand or capacity-based economics. Test scan patterns, concurrency, project controls, and data-location alignment. |
| [Databricks on Azure, AWS, or Google Cloud](https://docs.databricks.com/aws/en/getting-started/architecture) | Alternative to test. A candidate for mixed event/file engineering, SQL, and model workflows. Choose the cloud near the data and skills; validate actual feature/compute availability. |

### Industry benchmark and cost boundary

Reconcile time and invoice measures, change an ethical wall, and verify direct SQL and exported data obey the new access boundary. Use identical source scope, correction cases, service targets, and business definitions when comparing platforms.

Report cost per **reconciled matter reporting cycle and authorized dashboard refresh**, plus end-to-end freshness, concurrent-consumer response, reconciliation exceptions, and operator hours. Specifically measure permission administration, parallel restricted copies, report concurrency, document-engineering scope, and idle capacity.

Measure identity/semantic-model integration benefits; access inherited from office collaboration does not automatically implement legal ethical walls.


## A matter is the governing access boundary

A firm needs to understand work performed, recoverable value, billing progress, and client commitments without exposing one engagement to an unauthorized team. A global client hierarchy is useful for management, but it must not bypass matter-specific restrictions.

Start with work-in-progress and billing realization for one practice, using practice-management, time recording, billing, and approved ledger extracts. Document search, discovery processing, and generative legal research need their own content permissions and quality controls.

| Workflow | Evidence | Analytical use |
| --- | --- | --- |
| Accept an engagement | Client, matter, conflict clearance, engagement terms | Establish authorized scope and billing arrangements. |
| Staff and perform work | Matter role, time entry, task, fee-earner rate | Work and value by permitted team. |
| Review work in progress | Amendments, write-downs, approvals | Explain changes between recorded and recoverable work. |
| Bill and collect | Invoice lines, credit notes, receipts, allocation | Separate billing realization from cash collection. |
| Close and retain | Closure event, hold, retention authority | Limit continued access and preserve required evidence. |

## Model client relationships without flattening restrictions

Separate person, organization, client relationship, matter, engagement version, matter role, time entry, rate, invoice line, receipt allocation, and document reference. Preserve effective dates for fee arrangements, staffing, ethical walls, and rate changes.

The Raw Vault stores original time, billing, and amendment records. Users add Business Vault entities for rate selection, realization measures, partner/practice hierarchies, and approved allocation. Flow.BI output moves directly to FCT and TurboVault; review generated models before datavault4dbt/dbt execution.

A document reference is not authorization to extract its contents. Keep privileged documents, legal advice, witness material, and client identity behind the relevant matter permissions. Metadata such as matter names and counterparties can itself be sensitive and must be considered in Flow.BI's outbound metadata/fingerprint approval.

## Worked example: realization and collection answer different questions

Suppose recorded standard-value work is EUR 10,000 and approved billed fees are EUR 8,000. A defined billing realization is 80%. If EUR 6,000 has been collected, collection against those billed fees is 75%. Calling either measure simply “realization” hides the denominator.

Use **time entry × version** for work evidence, **invoice line × matter × currency** for billed value, and **receipt × invoice allocation** for collection. Keep taxes, disbursements, fixed fees, contingency fees, credit notes, and FX treatment explicit. A fixed-fee matter needs an approved allocation method rather than a guessed hourly conversion.

## Ethical walls must reach the data platform

Enforce matter access in query engines, semantic models, extracts, and caches. Test join paths that could reveal restricted counterparties through a client-wide report. Role removal or a new ethical wall must affect previously accessible analytical products within the agreed process.

Map the professional rules, engagement terms, privacy law, and privilege considerations for the jurisdictions involved. [ABA Formal Opinion 477R](https://www.americanbar.org/books/ecd/chapter/348777154/) is a US reference on protecting client communications; it is not a substitute for applicable local professional requirements. Azure, AWS, or Google Cloud deployment requires review of client contracts, operator access, retention, and vendor processing terms.

## Pilot with finance and the conflicts function

Practice management owns engagement meaning, finance owns value and ledger controls, the conflicts function owns wall definitions, and records staff owns retention/holds. Train modelers with a fee change, a credited invoice, a transferred matter, and an ethical-wall change.

Acceptance requires reconciliation to time and billing sources, reproducible rate selection, correct receipt allocations, and negative access tests on matter-level joins and exports. Demonstrate an access revocation and a legal hold. Do not publish a partner leaderboard before allocation and denominator rules are agreed.

Extend to capacity planning and approved knowledge products after financial and access controls work. Full technology, cloud, training, and reference guidance follows.

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

## License

Copyright © 2026 Scalefree International GmbH. The original text, tables, and diagrams in this blueprint are licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). Credit Scalefree, identify changes, and retain the applicable ShareAlike terms when distributing adaptations. See [LICENSE.md](LICENSE.md) for scope, attribution, and the governing legal code. Referenced software and third-party material retain their own licenses.
