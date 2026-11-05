# Data Platform Blueprints

Practical blueprints for building data platforms around the work an organization needs to perform: the source events it records, the decisions it supports, the calculations it must explain, and the controls it must demonstrate.

This collection contains **37 self-contained blueprints**: 16 industry blueprints focused on sovereignty, 16 industry blueprints focused on managed-cloud value and total cost of ownership, and five technology blueprints. Each document contains its implementation guidance, training plan, source links, and Next steps.

## Choose a starting point

| Series | Main decision | What the blueprint provides |
| --- | --- | --- |
| Sovereign industry | How should we retain control over data, processing, administration, portability, and exit? | A real industry workflow, worked calculation, Data Vault entities, domain controls, pilot, and open-source-first implementation on Azure, AWS, or Google Cloud. |
| Cloud industry | Which managed platform offers the best fit for our workload, team, delivery targets, and lifecycle cost? | The same business workflow with a conditional platform recommendation, alternatives, workload benchmark, TCO drivers, and managed operating design. |
| Technology | How should we implement and operate a selected platform? | Platform-specific architecture, table/compute choices, integration, cost drivers, recovery, migration, and acceptance evidence. |

Sovereignty and managed-cloud value are complementary decision dimensions. The cloud series prioritizes delivery speed, managed capability, elasticity, and useful platform integration. It retains privacy, security, contractual, and operational requirements. TCO is measured across implementation, consumption, licenses, staffing, recovery, and exit; a newer feature earns its place through a concrete use case and a validated benefit.

Platform recommendations are workload hypotheses, not universal industry rankings. Read the assumptions in each document before adopting a recommendation. An organization's cloud estate, skills, data gravity, service availability, pricing agreement, and operating capacity can change the decision.

## Industry table of contents

| Industry | Workflow focus | Sovereign blueprint | Cloud blueprint and recommended starting point |
| --- | --- | --- | --- |
| Higher education | Applications, cohorts, enrollment, and institutional reporting | [Sovereign Higher education](sovereign-higher-education-data-platform-blueprint.md) | [Microsoft Fabric on Azure](cloud-higher-education-data-platform-blueprint.md) |
| Banking | Parties, daily balances, ledger reconciliation, and reporting evidence | [Sovereign Banking](sovereign-banking-data-platform-blueprint.md) | [Snowflake on Azure](cloud-banking-data-platform-blueprint.md) |
| Insurance | Coverage, claims development, reserve snapshots, and recovery | [Sovereign Insurance](sovereign-insurance-data-platform-blueprint.md) | [Snowflake on AWS](cloud-insurance-data-platform-blueprint.md) |
| Healthcare networks | Cross-provider referrals, encounters, identity, and sharing | [Sovereign Healthcare networks](sovereign-healthcare-data-platform-blueprint.md) | [Snowflake on Azure](cloud-healthcare-data-platform-blueprint.md) |
| Hospitals | Patient flow, staffed capacity, ward stays, and clinical-system boundaries | [Sovereign Hospitals](sovereign-hospitals-data-platform-blueprint.md) | [Microsoft Fabric on Azure](cloud-hospitals-data-platform-blueprint.md) |
| Government | Service cases, statutory clocks, agency sharing, and public releases | [Sovereign Government](sovereign-government-data-platform-blueprint.md) | [Microsoft Fabric on Azure](cloud-government-data-platform-blueprint.md) |
| Energy and utilities | Meter intervals, substitutions, billing, settlement, and IT/OT boundaries | [Sovereign Energy and utilities](sovereign-energy-and-utilities-data-platform-blueprint.md) | [Databricks on Azure](cloud-energy-and-utilities-data-platform-blueprint.md) |
| Manufacturing | Lot/serial genealogy, execution, rework, quality, and material quantities | [Sovereign Manufacturing](sovereign-manufacturing-data-platform-blueprint.md) | [Databricks on Azure](cloud-manufacturing-data-platform-blueprint.md) |
| Nonprofit | Restricted funding, expenditure, program delivery, and outcome evidence | [Sovereign Nonprofit](sovereign-nonprofit-data-platform-blueprint.md) | [Google Cloud with BigQuery](cloud-nonprofit-data-platform-blueprint.md) |
| Agriculture | Field seasons, crop operations, moisture-adjusted yield, and delivery lots | [Sovereign Agriculture](sovereign-agriculture-data-platform-blueprint.md) | [Google Cloud with BigQuery](cloud-agriculture-data-platform-blueprint.md) |
| Legal services | Matters, ethical walls, work in progress, billing, and collection | [Sovereign Legal services](sovereign-legal-services-data-platform-blueprint.md) | [Microsoft Fabric on Azure](cloud-legal-services-data-platform-blueprint.md) |
| Travel retail and intermediaries | Booking amendments, supplier fulfillment, commission, and settlement | [Sovereign Travel retail and intermediaries](sovereign-travel-data-platform-blueprint.md) | [Google Cloud with BigQuery](cloud-travel-data-platform-blueprint.md) |
| Transportation and logistics | Handling units, transport legs, custody, promises, and delivered quantities | [Sovereign Transportation and logistics](sovereign-transportation-and-logistics-data-platform-blueprint.md) | [Databricks on AWS](cloud-transportation-and-logistics-data-platform-blueprint.md) |
| Airlines | Dated flight legs, passenger services, rebooking, and disruption chains | [Sovereign Airlines](sovereign-airline-data-platform-blueprint.md) | [Databricks on AWS](cloud-airline-data-platform-blueprint.md) |
| Integrated tour operators | Packages, committed flight/hotel capacity, service components, and group economics | [Sovereign Integrated tour operators](sovereign-integrated-tour-operators-data-platform-blueprint.md) | [Snowflake on AWS](cloud-integrated-tour-operators-data-platform-blueprint.md) |
| Academic, alumni, and civic/political organizations | Membership, chapters, events, volunteering, dues, and affiliation privacy | [Sovereign Academic, alumni, and civic/political organizations](sovereign-academic-alumni-and-civic-political-organizations-data-platform-blueprint.md) | [Google Cloud with BigQuery](cloud-academic-alumni-and-civic-political-organizations-data-platform-blueprint.md) |

Healthcare networks and hospitals have separate blueprints: one follows coordination across providers, while the other follows activity and capacity within a hospital. Travel retail, airlines, and integrated tour operators also have separate operating models. TUI and Fulbright Alumni e.V. illustrate organizational categories using linked public information; their examples do not describe private internal architectures.

## Technology table of contents

| Blueprint | Design emphasis |
| --- | --- |
| [Snowflake](snowflake-data-platform-blueprint.md) | SQL workload isolation, target-table/refresh ownership, consumption, and migration. |
| [Databricks](databricks-data-platform-blueprint.md) | Mixed engineering/SQL/model workloads, event replay, catalog governance, and operating cost. |
| [Microsoft Fabric](microsoft-fabric-data-platform-blueprint.md) | Warehouse/Lakehouse choice, SQL-to-Power BI delivery, capacity, and permissions. |
| [Google BigQuery](google-bigquery-data-platform-blueprint.md) | Managed SQL, partitioning/clustering, scan versus capacity economics, and workload controls. |
| [Apache Cloudberry](apache-cloudberry-data-platform-blueprint.md) | PostgreSQL/Greenplum lineage, distributed SQL, compatibility routes, distribution, and database operations. |

## Common implementation language

The modeling chain is **Flow.BI → FCT → TurboVault → generated-model review → datavault4dbt/dbt execution**. Users extend the overall model through additional **Business Vault entities**. **dlt** supplies supported ingestion routes; specialized source protocols require appropriate interfaces or gateways.

Flow.BI requires actual source data; source metadata is mostly optional and improves results. Its local-processing Docker image runs in customer-controlled Azure, AWS, or Google Cloud infrastructure. Derived metadata and statistical fingerprints use an approved connection to the Flow.BI backend.

**DWA** means Data Warehouse Automation. **DVA** refers exclusively to the Data Vault Alliance. The training sections include Data Vault, DWA/dbt, and Flow.BI learning paths. The documents link to Scalefree and Flow.BI implementation material and primary industry/platform references.

## Use a blueprint in a project

1. Choose the industry document closest to the actual workflow and identify a business owner.
2. Define source scope, grain, calculation, access, reporting cutoff, and reconciliation evidence.
3. Select the sovereignty or managed-cloud decision emphasis and test the strongest platform alternative.
4. Run the worked example and failure cases using representative, authorized data.
5. Demonstrate historical reconstruction, permissions, recovery, regeneration, and operating cost before expanding.

The worked numbers are illustrative, and the proposed controls and acceptance measures need project-specific targets. Apply the relevant legal/regulatory and professional requirements to the actual organization and jurisdiction. Pin the tool versions and verify integrations against those versions.

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
