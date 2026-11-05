# Cloud Hospitals Platform Selection Guide

**Connect hospital activity, capacity, clinical events, and finance to explain patient flow and operational performance.**


Compare Snowflake, Databricks, Microsoft Fabric and Google BigQuery for a hospitals data platform supporting staffed-bed utilization and patient flow. This selection guide evaluates workload fit, integration, governance and total cost; four linked implementation blueprints carry the platform-specific architecture and delivery plan.
Audience: hospital leadership, nursing operations, clinical informatics, theatre management, revenue cycle, privacy, and data teams. Updated: 5 November 2026.

## Standalone platform blueprints

- [Snowflake implementation](snowflake/cloud-hospitals-snowflake-data-platform-blueprint.md)
- [Databricks implementation](databricks/cloud-hospitals-databricks-data-platform-blueprint.md)
- [Microsoft Fabric implementation](microsoft-fabric/cloud-hospitals-microsoft-fabric-data-platform-blueprint.md)
- [Google BigQuery implementation](google-bigquery/cloud-hospitals-google-bigquery-data-platform-blueprint.md)

## From encounter to a governed Hospitals data product

**Source evidence and model inputs.** [dlt](https://github.com/dlt-hub/dlt) (data load tool) is the ingestion library for supported API, database, and file feeds. In this hospitals design, it brings admission/transfer/discharge, bed-management, and capacity data into controlled landing datasets with load evidence. [Flow.BI](https://flow.bi/) analyzes source values and relationships to generate the logical Data Vault structure. The modeling input includes encounter, ward stay, and bed; actual data is required, while available source metadata improves the result.

**Logical model to generated SQL.** For the hospitals model, [FCT](https://www.scalefree.com/consulting/turbovault/flowbi-connector/) (Flow.BI Connector for TurboVault4dbt) connects the modeling service to the generator by retrieving and transforming Flow.BI output. [TurboVault](https://www.scalefree.com/consulting/turbovault/) translates Data Vault metadata into the dbt model layer. Review those generated artifacts before executing the hospitals transformations; Studio is the online access option, requested through Scalefree.

**A maintained analytical product.** [datavault4dbt](https://github.com/ScalefreeCOM/datavault4dbt) supplies reusable loading logic for staging and the required Vault entity types. It contributes the technical history-loading foundation for encounter, ward stay, and bed. [dbt](https://github.com/dbt-labs/dbt) compiles model SQL, resolves dependencies, runs transformations, and executes tests on the target. User-defined Business Vault models add ward-stay intervals and staffed-capacity denominators within that workflow.

[Microsoft Fabric Warehouse](https://learn.microsoft.com/en-sg/fabric/fundamentals/decision-guide-lakehouse-warehouse) is the reference target for this hospitals architecture. Microsoft Fabric is the managed analytics platform; its Warehouse is the SQL target for staging, Vault models, and marts in this design. Power BI supplies the reporting/semantic delivery path. Azure is the cloud ecosystem for the Fabric deployment and the customer-controlled source/profiling environment; tenant identity, networking, storage, capacity, and execution need explicit owners. Size and operate these services against the hospitals pilot rather than a generic platform benchmark.

[Scalefree](https://www.scalefree.com/) combines data-platform advisory, implementation delivery, automation tooling, and team training. For this blueprint, that support can include the hospitals source-to-model design, implementation of ward-stay intervals and staffed-capacity denominators, and training the team to maintain the resulting product.


[DataHub](https://docs.datahub.com/docs/ui-ingestion) is the metadata catalog for this hospitals stack. It joins asset discovery, business definitions, ownership, and lineage so teams can trace encounter and understand the derived rules for ward-stay intervals and staffed-capacity denominators.

## Cloud data and model flow

```mermaid
flowchart TD
    SRC["Approved industry source systems"] --> ING["dlt in customer cloud runtime"]
    ING --> LAND["Controlled landing data"]
    subgraph PLATFORM["Microsoft Fabric on Azure"]
        LAND --> STG["Typed staging"]
        STG --> RV["Generated Raw Vault"]
        RV --> BV["Business Vault entities and rules"]
        BV --> PRODUCT["Industry metrics and analytical products"]
    end
    LAND --> LOCAL["Flow.BI profiling Docker in customer Azure, AWS or Google Cloud"]
    META["Optional source metadata"] --> LOCAL
    LOCAL -->|Derived metadata and statistical fingerprints| FLOW["Flow.BI backend: logical DV model"]
    FLOW --> FCT["FCT: retrieve and transform model output"]
    FCT --> TURBO["TurboVault: generate dbt models"]
    TURBO --> REVIEW["Review generated models"]
    REVIEW --> PROJECT["Versioned dbt project"]
    PROJECT --> RUN["datavault4dbt macros executed through dbt"]
    RUN -. transforms .-> STG
    RUN -. loads .-> RV
    USER["User-authored additional Business Vault entities"] --> CUSTOM["Versioned Business Vault models"]
    CUSTOM --> DBT["dbt: execute business rules and tests"]
    DBT -. builds .-> BV
    PROJECT -. supported artifact ingestion .-> CAT["DataHub: glossary, ownership and lineage"]
    PLATFORM -. supported warehouse metadata .-> CAT
    PRODUCT -. validated BI metadata .-> CAT
```

Actual landing data is required for Flow.BI profiling; source metadata enriches the result. Raw source records remain in the customer environment during local profiling; the documented backend exchange needs approved outbound connectivity. Flow.BI output goes directly to FCT and TurboVault. Review follows generation and precedes datavault4dbt/dbt execution. Additional Business Vault entities carry user-maintained business logic. The DataHub edges represent integrations to configure and test, including explicit lineage for unsupported steps; catalog registration does not grant data access. Platform alternatives below use the same logical flow with their tested ingestion and dbt adapters.

## Platform recommendation and decision assumptions

**Recommended starting point: Microsoft Fabric on Azure.** This recommendation assumes a hospital using Microsoft identity and Power BI for operational planning with a capable SQL analytics team. It is an architectural judgment to validate, not a claim that an industry has one universally best platform.

Ward capacity, patient flow, theatre planning, and finance reporting can share a governed semantic delivery path.

Consume approved clinical interfaces into landing, run the tested Warehouse Vault, and deliver purpose-restricted capacity and flow semantic products.

**Strongest alternatives:** Snowflake on Azure for independently scaled enterprise SQL reporting; Databricks for large streaming/imaging engineering workloads.

### Microsoft Fabric on Azure

[Microsoft Fabric](https://learn.microsoft.com/en-sg/fabric/fundamentals/decision-guide-lakehouse-warehouse) combines managed analytical services and Power BI delivery. For hospitals, use Warehouse as the tested SQL/dbt target when that is the selected route, and keep Lakehouse/file/Spark processing an explicit option with separate adapter/write tests. Azure supplies the surrounding customer identity, source-access and profiling environment.

Define ward-stay intervals and staffed-capacity denominators once in the agreed transformation/semantic layer and test the handoff to reports. Measure capacity contention during a real hospitals refresh and consumer burst. Include capacity, storage, applicable BI/user licensing, ingestion, and administration. Validate the dlt-to-landing and landing-to-Warehouse path before treating source ingestion as production-ready.

### Snowflake on Azure or AWS

[Snowflake](https://docs.snowflake.com/en/user-guide/cost-understanding-overall) provides the managed SQL warehouse for hospitals. Land approved source deliveries, build staging and the Raw Vault with the tested Snowflake adapter, and implement ward-stay intervals and staffed-capacity denominators as owned Business Vault models. Align the Azure or AWS account/region with the source estate, consumers, and operating skills.

Use distinct load, transformation, and reporting compute where the hospitals service windows need isolation. The evaluation must reconstruct historical encounter evidence and run concurrent consumers while applying a correction. Price compute, storage/retained copies, managed/serverless features used, transfer, and recovery; an AWS-to-Azure change is a migration decision with its own cost.

### Google Cloud with BigQuery

[BigQuery](https://cloud.google.com/bigquery/pricing) is the managed warehouse and SQL engine for a Google Cloud implementation. Use governed projects/datasets, supported ingestion, a tested BigQuery adapter, and explicit partition/cluster choices for encounter, ward stay, bed, capacity interval, and procedure. Customer-controlled Google Cloud services also host source landing and the Flow.BI profiling component.

Benchmark staffed-bed utilization and patient flow with representative data and historical corrections. Compare on-demand analysis with capacity-based execution under the same refresh and concurrency targets. Measure scanned data/capacity, storage, load/streaming features actually used, transfer, BI, and operator time. Predicate pruning and repeat-query behavior must be demonstrated with this industry's facts.

### Databricks on Azure, AWS, or Google Cloud

[Databricks](https://docs.databricks.com/aws/en/getting-started/architecture) contributes engineering and SQL execution over governed lakehouse tables. Use the approved incumbent cloud for storage, network and identity, then retain raw deliveries, typed source tables, the Raw Vault, and ward-stay intervals and staffed-capacity denominators with an owned writer/refresh contract. Unity Catalog governs the platform assets; DataHub provides the cross-stack business discovery and lineage layer in this cloud edition.

Evaluate ingestion/replay and SQL serving for hospitals together. Preserve the fact grain and corrections while comparing job/SQL/serverless choices supported by the chosen cloud and edition. Include platform usage, underlying infrastructure where separately billed, table maintenance, storage, network, retries, and retained environments. A streaming or AI feature needs a measurable product use case.

### Ingestion contract for the platform comparison

**Primary ingestion: dlt (open-source library).** The hospitals team owns source code/configuration, compatible drivers/destinations, extraction state, packaging, schedules and recovery. Preserve source event/version evidence and prove deletes, late records and schema changes. Include engineering, execution and maintenance in TCO. Fivetran is a managed commercial alternative when documented connectors and a complete cost comparison support it.

Use dlt only through a supported source/destination or a validated landing-to-target load path. Verify Fabric Warehouse versus Lakehouse endpoints separately; matching a SQL protocol or writing files does not establish complete target compatibility.


### Industry benchmark and cost boundary

Run morning operational dashboards while reconstructing bed intervals after a late discharge and a staffed-capacity correction. Use identical source scope, correction cases, service targets, and business definitions when comparing platforms.

Report cost per **validated ward-day product and operational reporting refresh**, plus end-to-end freshness, concurrent-consumer response, reconciliation exceptions, and operator hours. Specifically measure refresh frequency, morning dashboard concurrency, repeated interval joins, and semantic-model duplication.

Test the chosen Warehouse/semantic-model mode with actual concurrency; platform freshness must not be mistaken for a validated bedside decision system.


## Follow a patient through the hospital

Hospital operations involve admission, transfers, procedures, diagnostics, and discharge, often recorded in different systems. The platform must preserve event order and corrections while reconciling patient encounters with physical capacity. It supports planning and retrospective analysis; bedside systems, device control, prescribing, and emergency decisions remain within validated clinical workflows.

Start with staffed-bed utilization and inpatient flow for one hospital and selected wards. Include admission/discharge/transfer events, bed-management history, staffing-capacity declarations, and encounter master data. The broader healthcare blueprint addresses cross-provider identity and referrals; this blueprint concentrates on in-hospital operations.

| Operational step | Primary evidence | Question answered |
| --- | --- | --- |
| Admission | Encounter registration and actual arrival | When did the patient enter the measured inpatient scope? |
| Bed assignment and transfer | Assignment intervals, ward changes, correction events | Which ward and bed were occupied, and when? |
| Procedure or diagnostic work | Theatre case, laboratory, radiology, encounter links | Which service events contributed to flow? |
| Discharge readiness and departure | Readiness status, discharge order, actual departure | Separate clinically ready, administratively discharged, and physically departed states. |
| Billing and close | Coding, charge capture, payer and ledger controls | Reconcile activity to billable and recorded financial events. |

## Build encounter and location history before dashboards

Use a governed patient-identity crosswalk and preserve merge/unmerge events. Distinguish patient, encounter, episode, ward, physical bed, staffed capacity interval, procedure, order, specimen, and claim/invoice. A transferred patient can create multiple ward stays inside one encounter.

Raw Vault entities preserve original admission/transfer/discharge events and amendments. Add Business Vault entities for ward-stay intervals, encounter episodes, staffed-capacity intervals, operational calendars, and exclusions. Flow.BI output reaches FCT and TurboVault directly; review generated models after TurboVault and before macro execution.

Use existing clinical interface infrastructure and documented [FHIR](https://hl7.org/fhir/overview.html) resources where supported. Pin interface versions and validate resource history, repeated fields, and reference resolution. Do not treat a database extract as a substitute for agreed clinical event semantics.

## Worked example: staffed capacity and occupancy

A ward declares 20 staffed beds for a 24-hour day: 480 staffed bed-hours. If validated occupancy intervals total 360 hours, an illustrative occupancy measure is 75%. Physical installed capacity may be larger; it is a different denominator.

Define how leave, temporary closure, boarding, observation, and overlapping assignments affect the metric. If a bed is closed for part of a day, use its effective capacity interval. Compute durations from instants consistently and explain local-day aggregation across daylight-saving changes.

The analytical grain is **encounter × ward-stay interval**, joined to **ward × staffed-capacity interval** through controlled interval logic. Retain interval-building rule versions and report cutoff. Test both overlap and gaps rather than filling them silently.

## Keep clinical, workforce, and financial permissions distinct

Patient-level flow data requires authorized operational access. Clinical content, staff rosters, sensitive specialties, and billing identifiers need their own field and purpose restrictions. Treat demographic matching, hospital discharge status, and clinical death records with explicit source precedence and escalation.

Map relevant [GDPR](https://eur-lex.europa.eu/eli/reg/2016/679/oj/eng) and local health-record requirements. For US entities in scope, evaluate [HIPAA roles and business-associate arrangements](https://www.hhs.gov/hipaa/for-professionals/covered-entities/index.html). Pseudonymized analytics still needs governance. Protect small specialty/diagnosis aggregates in exports.

Run Flow.BI's local Docker component within the hospital-controlled Azure, AWS, or Google Cloud environment. Approve the outbound fingerprint/metadata payload and source access without introducing workload or availability problems for clinical systems.

## Pilot with nursing operations and clinical informatics

Nursing operations approves capacity semantics, informatics validates clinical events, ward managers resolve assignment exceptions, and finance defines activity-to-revenue boundaries. Training should include an admission correction, a ward transfer, a staffed-bed closure, and a patient unmerge.

Release criteria include reconciliation of encounters to the source system, sampled agreement of reconstructed stays with bed-management evidence, verified denominator changes, source-to-metric lineage, and explicit handling of late discharge events. A patient transfer must neither double-count bed occupancy nor lose the encounter.

Extend later to theatre utilization, diagnostic turnaround, discharge delays, and revenue-cycle products. Define separate denominators and clinical ownership for each. Complete technology, deployment, training, and reference guidance follows.

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

## Metadata stewardship in the hospitals stack

[DataHub](https://docs.datahub.com/docs/ui-ingestion) is the metadata catalog for this hospitals stack. It joins asset discovery, business definitions, ownership, and lineage so teams can trace encounter and understand the derived rules for ward-stay intervals and staffed-capacity denominators.

For this product, register **encounter, ward stay, bed, capacity interval, and procedure** as governed concepts linked to real assets. Describe **ward-stay intervals and staffed-capacity denominators** with an owner, calculation/rule version, eligible population, cutoff, and known limitations. Publish only the metadata fields that the catalog users are authorized to see.

### Deployment and processing boundary

Use DataHub Core in a customer-managed environment as the reference open-source catalog, or choose DataHub Cloud under an explicit service agreement. Budget catalog infrastructure/search, upgrades, credentials, metadata collection, and stewardship; a managed catalog adds its subscription and provider dependencies. This catalog complements the database's native access controls and, where present, Unity Catalog or a table-format metastore.

### Connect execution evidence to meaning

Collect [dbt artifacts and target-warehouse metadata](https://docs.datahub.com/docs/generated/ingestion/sources/dbt) together so model nodes resolve to the actual tables/views. Select supported warehouse/BI sources and validate the specific Fabric or Cloudberry route where relevant. For Fivetran-led ingestion, assess the [DataHub Fivetran source](https://docs.datahub.com/docs/generated/ingestion/sources/fivetran) for connector metadata/lineage coverage. Use explicit API-emitted lineage for unrepresented steps rather than assuming the full source-to-report path is automatic.

dbt remains the execution/test layer; DataHub collects the supported metadata and quality results. Test which table/column lineage, test status, and deletion behavior are captured by the actual sources and artifact versions.

Catalog acceptance follows the hospitals pilot: trace an approved source field through a generated model and Business Vault extension to a published metric, then test a schema change, a stale scan, and a permission revocation. Mask sample values and sensitive SQL/descriptions where required. Track scan coverage and unresolved lineage gaps explicitly.

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
| Catalog stewardship | [DataHub documentation](https://docs.datahub.com/docs/ui-ingestion) | Data owners, stewards, catalog operators, and engineers. | Document encounter, approve definitions, trace model lineage, and operate metadata scans and access. |

**Terminology:** DWA means Data Warehouse Automation. The DWA training track uses Scalefree's published automation learning blocks. DVA refers exclusively to the Data Vault Alliance.

The Data Vault certification training covers Data Vault 2.1. Align the curriculum with the selected tool versions and implementation patterns. Its dbt offering is customizable; agree the agenda and prerequisites before booking.
Flow.BI In-Depth requires Flow.BI Introduction and general data-architecture knowledge. Generated Data Vault Models requires Flow.BI Introduction and Data Vault modeling knowledge. Use the [Flow.BI training catalog](https://flow.bi/trainings/) to select classes or a bundle and confirm current delivery details.

Recommended sequence:

1. Run DV foundations and modeling to understand the generated Raw Vault and design Business Vault extensions.
2. Complete Flow.BI Introduction before connecting the pilot sources; follow with In-Depth for the configuration and operations team and Generated Data Vault Models for modelers and engineers.
3. Run dbt fundamentals before building the first transformation pipeline.
4. Run DWA practical sessions when the first Flow.BI output is available for direct processing through FCT and TurboVault. Include metadata presentation training for the team publishing model documentation.
5. Complete an organization-led lab using synthetic datasets for the selected industry workflow: dlt ingestion, Flow.BI generation, direct FCT transfer, TurboVault generation, generated-model review, datavault4dbt/dbt execution, user-added Business Vault entities, and industry-specific reconciliation.
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
| PostgreSQL workload assessment | [Architectural Limitations of PostgreSQL for Enterprise Data Vault Workloads](https://www.scalefree.com/blog/data-vault/architectural-limitations-of-postgresql-for-enterprise-data-vault-workloads/) | Explains why a bounded PostgreSQL pilot is not a large-scale Data Vault recommendation. Compare the selected platform with Cloudberry MPP and its per-table row/column storage where an open relational warehouse is required. |
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

## Choosing a cloud data platform for hospitals

### Which cloud data warehouse fits hospitals?

Begin with the workload for **staffed-bed utilization and patient flow** and the actual source estate. Compare SQL/reporting demand, event engineering, semantic delivery, historical corrections and the incumbent cloud. The recommendation in this guide is conditional; test the strongest alternative before selecting the platform.

### How should hospitals teams compare platform costs?

Run the same encounter, ward stay, and bed scope, reconciliation cases, refresh target and concurrent consumers on the candidates. Include migration, ingestion, compute, storage, transfer, BI, catalog operations, staffing, recovery and exit. A lower consumption bill is not a saving if correctness, service quality or required operating work changes.

## Related blueprints

- [Sovereign hospitals data platform](../sovereign/sovereign-hospitals-data-platform-blueprint.md)
- [Hospitals on Snowflake](snowflake/cloud-hospitals-snowflake-data-platform-blueprint.md)
- [Hospitals on Databricks](databricks/cloud-hospitals-databricks-data-platform-blueprint.md)
- [Hospitals on Microsoft Fabric](microsoft-fabric/cloud-hospitals-microsoft-fabric-data-platform-blueprint.md)
- [Hospitals on Google BigQuery](google-bigquery/cloud-hospitals-google-bigquery-data-platform-blueprint.md)
- [All industry and technology blueprints](../../../README.md)

## Next steps

Validate the [DataHub metadata integration](https://docs.datahub.com/docs/ui-ingestion) and include catalog deployment, licensing/operations, stewardship, and source-to-report lineage in the architecture discussion.


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

Copyright © 2026 Scalefree International GmbH. The original text, tables, and diagrams in this blueprint are licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). Credit Scalefree, identify changes, and retain the applicable ShareAlike terms when distributing adaptations. See [LICENSE.md](../../../LICENSE.md) for scope, attribution, and the governing legal code. Referenced software and third-party material retain their own licenses.
