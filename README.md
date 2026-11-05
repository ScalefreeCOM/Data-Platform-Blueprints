# Data Platform Blueprints

Practical blueprints for building data platforms around the work an organization needs to perform: the source events it records, the decisions it supports, the calculations it must explain, and the controls it must demonstrate.

This collection contains **137 self-contained blueprints**: 66 industry and specialist-sector blueprints focused on sovereignty, 66 focused on managed-cloud value and total cost of ownership, and five technology blueprints. Each document contains its implementation guidance, training plan, source links, and Next steps.

[Browse industries](#browse-industries) · [Browse technologies](#browse-technologies) · [Tools and Scalefree](#tools-platform-choices-and-scalefree) · [Next steps](#next-steps) · [License](#license-and-attribution)

## Choose a starting point

| Series | Main decision | What the blueprint provides |
| --- | --- | --- |
| Sovereign industry | How should we retain control over data, processing, administration, portability, and exit? | A real industry workflow, worked calculation, Data Vault entities, domain controls, pilot, and open-source-first implementation on Azure, AWS, or Google Cloud. |
| Cloud industry | Which managed platform offers the best fit for our workload, team, delivery targets, and lifecycle cost? | The same business workflow with a conditional platform recommendation, alternatives, workload benchmark, TCO drivers, and managed operating design. |
| Technology | How should we implement and operate a selected platform? | Platform-specific architecture, table/compute choices, integration, cost drivers, recovery, migration, and acceptance evidence. |

Sovereignty and managed-cloud value are complementary decision dimensions. The cloud series prioritizes delivery speed, managed capability, elasticity, and useful platform integration. It retains privacy, security, contractual, and operational requirements. TCO is measured across implementation, consumption, licenses, staffing, recovery, and exit; a newer feature earns its place through a concrete use case and a validated benefit.

Platform recommendations are workload hypotheses, not universal industry rankings. Read the assumptions in each document before adopting a recommendation. An organization's cloud estate, skills, data gravity, service availability, pricing agreement, and operating capacity can change the decision.

## Browse industries

Choose a sector group, then open either edition for an industry. **Sovereign** emphasizes control, portability, and the processing boundary. **Cloud** emphasizes managed-platform fit, delivery, and total cost of ownership; its link names the recommended starting platform.

[Finance and insurance](#finance-and-insurance) · [Healthcare and life sciences](#healthcare-and-life-sciences) · [Government, education, and community](#government-education-and-community) · [Manufacturing and industrial products](#manufacturing-and-industrial-products) · [Energy, environment, and natural resources](#energy-environment-and-natural-resources) · [Retail, distribution, and food](#retail-distribution-and-food) · [Travel and hospitality](#travel-and-hospitality) · [Transportation and infrastructure](#transportation-and-infrastructure) · [Property, construction, and facilities](#property-construction-and-facilities) · [Technology, telecommunications, and space](#technology-telecommunications-and-space) · [Media, marketing, and entertainment](#media-marketing-and-entertainment) · [Professional and workforce services](#professional-and-workforce-services)

### Finance and insurance

- **Asset Management** — Explain how portfolio positions, trades, prices, and cash produce an approved valuation and investor report. [Sovereign](sovereign-asset-management-data-platform-blueprint.md) · [Cloud: Snowflake on AWS](cloud-asset-management-data-platform-blueprint.md)
- **Banking** — Parties, daily balances, ledger reconciliation, and reporting evidence. [Sovereign](sovereign-banking-data-platform-blueprint.md) · [Cloud: Snowflake on Azure](cloud-banking-data-platform-blueprint.md)
- **Capital Markets and Exchanges** — Reconstruct order-to-execution history and market-quality measures without losing cancellations or sequence gaps. [Sovereign](sovereign-capital-markets-and-exchanges-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-capital-markets-and-exchanges-data-platform-blueprint.md)
- **Insurance** — Coverage, claims development, reserve snapshots, and recovery. [Sovereign](sovereign-insurance-data-platform-blueprint.md) · [Cloud: Snowflake on AWS](cloud-insurance-data-platform-blueprint.md)
- **Payments and Merchant Acquiring** — Explain each merchant payout from authorized transactions, captures, refunds, fees, reserves, and disputes. [Sovereign](sovereign-payments-and-merchant-acquiring-data-platform-blueprint.md) · [Cloud: Snowflake on AWS](cloud-payments-and-merchant-acquiring-data-platform-blueprint.md)
- **Pension Funds** — Connect service history, contributions, benefit rules, investment assets, and payments while preserving member entitlements. [Sovereign](sovereign-pension-funds-data-platform-blueprint.md) · [Cloud: Snowflake on Azure](cloud-pension-funds-data-platform-blueprint.md)

### Healthcare and life sciences

- **Biotechnology and Clinical Research** — Connect study versions, participant visits, specimens, assays, and analysis snapshots while preserving blinding and protocol meaning. [Sovereign](sovereign-biotechnology-and-clinical-research-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-biotechnology-and-clinical-research-data-platform-blueprint.md)
- **Healthcare networks** — Cross-provider referrals, encounters, identity, and sharing. [Sovereign](sovereign-healthcare-data-platform-blueprint.md) · [Cloud: Snowflake on Azure](cloud-healthcare-data-platform-blueprint.md)
- **Hospitals** — Patient flow, staffed capacity, ward stays, and clinical-system boundaries. [Sovereign](sovereign-hospitals-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-hospitals-data-platform-blueprint.md)
- **Medical Devices** — Trace device design versions, serialized manufacture, distribution, complaints, and corrective-action evidence. [Sovereign](sovereign-medical-devices-data-platform-blueprint.md) · [Cloud: Snowflake on Azure](cloud-medical-devices-data-platform-blueprint.md)
- **Pharmaceuticals** — Connect material genealogy, batch execution, laboratory evidence, quality disposition, and distribution without displacing validated release authority. [Sovereign](sovereign-pharmaceuticals-data-platform-blueprint.md) · [Cloud: Snowflake on Azure](cloud-pharmaceuticals-data-platform-blueprint.md)
- **Social Care** — Connect referral, assessment, care plan, scheduled support, delivered visits, safeguarding boundaries, and funding. [Sovereign](sovereign-social-care-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-social-care-data-platform-blueprint.md)
- **Veterinary Services** — Connect animal identity, owner roles, encounters, treatment, diagnostics, medicines, and practice revenue. [Sovereign](sovereign-veterinary-services-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-veterinary-services-data-platform-blueprint.md)

### Government, education, and community

- **Academic, alumni, and civic/political organizations** — Membership, chapters, events, volunteering, dues, and affiliation privacy. [Sovereign](sovereign-academic-alumni-and-civic-political-organizations-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-academic-alumni-and-civic-political-organizations-data-platform-blueprint.md)
- **Government** — Service cases, statutory clocks, agency sharing, and public releases. [Sovereign](sovereign-government-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-government-data-platform-blueprint.md)
- **Higher education** — Applications, cohorts, enrollment, and institutional reporting. [Sovereign](sovereign-higher-education-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-higher-education-data-platform-blueprint.md)
- **Museums and Cultural Heritage** — Connect collection identity, provenance, location, conservation, loans, exhibitions, and authorized public discovery. [Sovereign](sovereign-museums-and-cultural-heritage-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-museums-and-cultural-heritage-data-platform-blueprint.md)
- **Nonprofit** — Restricted funding, expenditure, program delivery, and outcome evidence. [Sovereign](sovereign-nonprofit-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-nonprofit-data-platform-blueprint.md)
- **Public Safety and Emergency Services** — Connect incident intake, dispatch, resource status, arrival, handover, and review while preserving sensitive case boundaries. [Sovereign](sovereign-public-safety-and-emergency-services-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-public-safety-and-emergency-services-data-platform-blueprint.md)
- **Research Laboratories** — Connect sample custody, instrument runs, methods, calibration, results, and publication or customer deliverables. [Sovereign](sovereign-research-laboratories-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-research-laboratories-data-platform-blueprint.md)

### Manufacturing and industrial products

- **Aerospace and Defense Manufacturing** — Trace serialized configuration, approved design changes, conformity evidence, and delivery through a controlled production program. [Sovereign](sovereign-aerospace-and-defense-manufacturing-data-platform-blueprint.md) · [Cloud: Snowflake on Azure](cloud-aerospace-and-defense-manufacturing-data-platform-blueprint.md)
- **Automotive** — Connect vehicle configuration, production genealogy, delivery, warranty, and field-service history. [Sovereign](sovereign-automotive-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-automotive-data-platform-blueprint.md)
- **Chemicals** — Explain batch transformation, assay-dependent quantities, tank movements, quality disposition, and customer deliveries. [Sovereign](sovereign-chemicals-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-chemicals-data-platform-blueprint.md)
- **Electronics** — Connect component procurement, board assembly, firmware configuration, testing, and repair genealogy. [Sovereign](sovereign-electronics-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-electronics-data-platform-blueprint.md)
- **Manufacturing** — Lot/serial genealogy, execution, rework, quality, and material quantities. [Sovereign](sovereign-manufacturing-data-platform-blueprint.md) · [Cloud: Databricks on Azure](cloud-manufacturing-data-platform-blueprint.md)
- **Semiconductors** — Connect wafer fabrication history, equipment runs, test results, die mapping, packaging, and final yield. [Sovereign](sovereign-semiconductors-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-semiconductors-data-platform-blueprint.md)

### Energy, environment, and natural resources

- **Agriculture** — Field seasons, crop operations, moisture-adjusted yield, and delivery lots. [Sovereign](sovereign-agriculture-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-agriculture-data-platform-blueprint.md)
- **Energy and utilities** — Meter intervals, substitutions, billing, settlement, and IT/OT boundaries. [Sovereign](sovereign-energy-and-utilities-data-platform-blueprint.md) · [Cloud: Databricks on Azure](cloud-energy-and-utilities-data-platform-blueprint.md)
- **Fisheries and Aquaculture** — Connect stock/cohort origin, husbandry or catch events, biomass estimates, harvest, processing, and deliveries. [Sovereign](sovereign-fisheries-and-aquaculture-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-fisheries-and-aquaculture-data-platform-blueprint.md)
- **Forestry and Timber** — Connect forest compartments, harvest rights, log identity, mill transformation, grade, and timber deliveries. [Sovereign](sovereign-forestry-and-timber-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-forestry-and-timber-data-platform-blueprint.md)
- **Mining and Metals** — Connect ore extraction, grade estimates, stockpile movement, processing recovery, product assays, and shipment. [Sovereign](sovereign-mining-and-metals-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-mining-and-metals-data-platform-blueprint.md)
- **Oil and Gas** — Connect well production, measurement, gathering networks, allocation, ownership, and commercial settlement. [Sovereign](sovereign-oil-and-gas-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-oil-and-gas-data-platform-blueprint.md)
- **Renewable Energy Developers** — Connect project rights, construction milestones, commissioned assets, generation, curtailment, and contracted revenue. [Sovereign](sovereign-renewable-energy-developers-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-renewable-energy-developers-data-platform-blueprint.md)
- **Waste and Recycling** — Trace collections, weighed loads, sorting, recovered outputs, contamination, and customer settlement. [Sovereign](sovereign-waste-and-recycling-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-waste-and-recycling-data-platform-blueprint.md)
- **Water and Wastewater** — Connect abstraction/inflow, treatment, network supply, customer metering, quality observations, and discharge reporting. [Sovereign](sovereign-water-and-wastewater-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-water-and-wastewater-data-platform-blueprint.md)

### Retail, distribution, and food

- **E-commerce Marketplaces** — Explain marketplace orders, seller fulfillment, returns, commissions, and payouts across multiple commercial principals. [Sovereign](sovereign-e-commerce-marketplaces-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-e-commerce-marketplaces-data-platform-blueprint.md)
- **Food and Beverage Production** — Trace ingredients through processing, quality release, packaging, and shipment while reconciling yield and waste. [Sovereign](sovereign-food-and-beverage-production-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-food-and-beverage-production-data-platform-blueprint.md)
- **Restaurants and Catering** — Explain menu-item contribution and food waste from purchasing through preparation, service, and inventory. [Sovereign](sovereign-restaurants-and-catering-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-restaurants-and-catering-data-platform-blueprint.md)
- **Retail** — Connect store sales, returns, stock movements, promotions, and replenishment with stable product and location histories. [Sovereign](sovereign-retail-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-retail-data-platform-blueprint.md)
- **Wholesale Distribution** — Connect supplier procurement, warehouse inventory, customer allocations, deliveries, and rebates. [Sovereign](sovereign-wholesale-distribution-data-platform-blueprint.md) · [Cloud: Snowflake on Azure](cloud-wholesale-distribution-data-platform-blueprint.md)

### Travel and hospitality

- **Cruise Lines** — Connect sailing capacity, cabins, guests, onboard services, itinerary changes, and voyage economics. [Sovereign](sovereign-cruise-lines-data-platform-blueprint.md) · [Cloud: Snowflake on AWS](cloud-cruise-lines-data-platform-blueprint.md)
- **Hotels and Accommodation** — Connect reservations, room inventory, stays, service charges, and revenue through a defensible night-based model. [Sovereign](sovereign-hotels-and-accommodation-data-platform-blueprint.md) · [Cloud: Snowflake on AWS](cloud-hotels-and-accommodation-data-platform-blueprint.md)
- **Integrated tour operators** — Packages, committed flight/hotel capacity, service components, and group economics. [Sovereign](sovereign-integrated-tour-operators-data-platform-blueprint.md) · [Cloud: Snowflake on AWS](cloud-integrated-tour-operators-data-platform-blueprint.md)
- **Travel retail and intermediaries** — Booking amendments, supplier fulfillment, commission, and settlement. [Sovereign](sovereign-travel-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-travel-data-platform-blueprint.md)

### Transportation and infrastructure

- **Airlines** — Dated flight legs, passenger services, rebooking, and disruption chains. [Sovereign](sovereign-airline-data-platform-blueprint.md) · [Cloud: Databricks on AWS](cloud-airline-data-platform-blueprint.md)
- **Airports** — Connect flight movements, stand/gate occupancy, passenger-processing demand, baggage, and airport commercial services. [Sovereign](sovereign-airports-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-airports-data-platform-blueprint.md)
- **Maritime Shipping and Ports** — Connect voyage, port call, berth, cargo handling, customs-facing references, and terminal dwell evidence. [Sovereign](sovereign-maritime-shipping-and-ports-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-maritime-shipping-and-ports-data-platform-blueprint.md)
- **Rail Operators** — Connect planned timetable paths, actual train runs, rolling stock, passenger/freight services, and disruption recovery. [Sovereign](sovereign-rail-operators-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-rail-operators-data-platform-blueprint.md)
- **Transportation and logistics** — Handling units, transport legs, custody, promises, and delivered quantities. [Sovereign](sovereign-transportation-and-logistics-data-platform-blueprint.md) · [Cloud: Databricks on AWS](cloud-transportation-and-logistics-data-platform-blueprint.md)

### Property, construction, and facilities

- **Architecture and Engineering** — Explain design deliverables, revisions, approvals, effort, and change exposure across multidisciplinary projects. [Sovereign](sovereign-architecture-and-engineering-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-architecture-and-engineering-data-platform-blueprint.md)
- **Construction** — Connect contract scope, quantities, site progress, variations, commitments, and payment certification for one project. [Sovereign](sovereign-construction-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-construction-data-platform-blueprint.md)
- **Real Estate** — Explain property income, occupancy, lease obligations, and operating cost across ownership and management structures. [Sovereign](sovereign-real-estate-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-real-estate-data-platform-blueprint.md)
- **Security and Facilities Management** — Connect service obligations, sites/assets, planned work, incidents, authorized attendance, and customer billing. [Sovereign](sovereign-security-and-facilities-management-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-security-and-facilities-management-data-platform-blueprint.md)

### Technology, telecommunications, and space

- **Data Centers** — Connect tenant capacity, electrical/thermal metering, asset maintenance, incidents, and commercial service commitments. [Sovereign](sovereign-data-centers-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-data-centers-data-platform-blueprint.md)
- **IT and Managed Services** — Connect client service commitments, configuration items, changes, incidents, effort, and contract economics. [Sovereign](sovereign-it-and-managed-services-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-it-and-managed-services-data-platform-blueprint.md)
- **Software and SaaS** — Connect tenant contracts, entitlements, product usage, billing, support, and service reliability without crossing tenant boundaries. [Sovereign](sovereign-software-and-saas-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-software-and-saas-data-platform-blueprint.md)
- **Space and Satellite Operators** — Connect mission configuration, planned observations, ground contacts, received telemetry, processing, and customer products. [Sovereign](sovereign-space-and-satellite-operators-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-space-and-satellite-operators-data-platform-blueprint.md)
- **Telecommunications** — Connect service orders, network inventory, provisioning, usage, incidents, and charging for an accountable service lifecycle. [Sovereign](sovereign-telecommunications-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-telecommunications-data-platform-blueprint.md)

### Media, marketing, and entertainment

- **Advertising and Marketing Services** — Connect campaign commitments, placements, creative versions, exposure, conversion evidence, spend, and client reporting. [Sovereign](sovereign-advertising-and-marketing-services-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-advertising-and-marketing-services-data-platform-blueprint.md)
- **Film and Television Production** — Connect production commitments, shooting activity, asset versions, delivery obligations, and rights accounting. [Sovereign](sovereign-film-and-television-production-data-platform-blueprint.md) · [Cloud: Snowflake on AWS](cloud-film-and-television-production-data-platform-blueprint.md)
- **Gaming** — Connect player accounts, game sessions, virtual economies, purchases, experiments, and live-service reliability. [Sovereign](sovereign-gaming-data-platform-blueprint.md) · [Cloud: Databricks on Azure, AWS, or Google Cloud](cloud-gaming-data-platform-blueprint.md)
- **Media and Publishing** — Connect content rights, editions, distribution, audiences, subscriptions, and royalty/revenue obligations. [Sovereign](sovereign-media-and-publishing-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-media-and-publishing-data-platform-blueprint.md)
- **Sports and Live Events** — Connect event inventory, ticket rights, attendance, hospitality, sponsorship fulfillment, and venue economics. [Sovereign](sovereign-sports-and-live-events-data-platform-blueprint.md) · [Cloud: Google Cloud with BigQuery](cloud-sports-and-live-events-data-platform-blueprint.md)

### Professional and workforce services

- **Legal services** — Matters, ethical walls, work in progress, billing, and collection. [Sovereign](sovereign-legal-services-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-legal-services-data-platform-blueprint.md)
- **Professional Consulting** — Connect proposals, statements of work, staffing, effort, deliverables, fees, and project economics. [Sovereign](sovereign-professional-consulting-data-platform-blueprint.md) · [Cloud: Microsoft Fabric on Azure](cloud-professional-consulting-data-platform-blueprint.md)
- **Recruitment and Staffing** — Connect candidate consent, vacancies, applications, placements, assignments, approved time, payroll, and client billing. [Sovereign](sovereign-recruitment-and-staffing-data-platform-blueprint.md) · [Cloud: Snowflake on Azure](cloud-recruitment-and-staffing-data-platform-blueprint.md)

Healthcare networks and hospitals have separate blueprints: one follows coordination across providers, while the other follows activity and capacity within a hospital. Travel retail, airlines, and integrated tour operators also have separate operating models. TUI and Fulbright Alumni e.V. illustrate organizational categories using linked public information; their examples do not describe private internal architectures.

## Browse technologies

| Blueprint | Design emphasis |
| --- | --- |
| [Snowflake](snowflake-data-platform-blueprint.md) | SQL workload isolation, target-table/refresh ownership, consumption, and migration. |
| [Databricks](databricks-data-platform-blueprint.md) | Mixed engineering/SQL/model workloads, event replay, catalog governance, and operating cost. |
| [Microsoft Fabric](microsoft-fabric-data-platform-blueprint.md) | Warehouse/Lakehouse choice, SQL-to-Power BI delivery, capacity, and permissions. |
| [Google BigQuery](google-bigquery-data-platform-blueprint.md) | Managed SQL, partitioning/clustering, scan versus capacity economics, and workload controls. |
| [Apache Cloudberry](apache-cloudberry-data-platform-blueprint.md) | PostgreSQL/Greenplum lineage, distributed SQL, compatibility routes, distribution, and database operations. |

## Tools, platform choices, and Scalefree

Each blueprint introduces its tools in the context of the article's workflow. The introductions connect ingestion, data-driven modeling, connector handoff, project generation, macro-based loading, SQL execution, database storage/compute, and cloud infrastructure. Layouts vary between prose, operating steps, component lists, and compact responsibility tables.

[Scalefree](https://www.scalefree.com/) provides data and AI consulting, platform architecture and implementation, Data Vault automation tooling, and team training. The articles explain how that support relates to the proposed industry product or selected technology.

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

## License and attribution

Copyright © 2026 Scalefree International GmbH. The original blueprint text, tables, and diagrams are available under [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).

You may share and adapt them, including commercially, under the license terms: give appropriate credit, link to the license, identify changes, and apply the required ShareAlike terms to shared adaptations. Software, referenced standards/documentation, and other third-party material retain their own licenses. See [LICENSE.md](LICENSE.md) for the scope, an attribution example, and the governing legal code.
