# SPFS Pro — Smart Personal Finance System

> **An AI-native personal finance operating system that helps people understand, plan, and improve their financial lives.**

SPFS Pro is being designed to move beyond retrospective expense tracking toward a trusted personal financial decision-support system: one that consolidates financial information, explains current conditions, forecasts plausible outcomes, identifies risks, and helps users take informed action.

**Project status:** Early-stage / architecture and product definition. The repository is currently being established. Technologies and capabilities described in the architecture documents are proposed target-state decisions, not claims that production systems have already been implemented.

## Vision

SPFS Pro aims to act as an **AI CFO for individuals**. It should answer:

- Where does my money go, and what has changed?
- Can I safely meet my upcoming obligations?
- What should I prioritize next: liquidity, expensive debt, savings, or investing?
- How resilient are my finances to income loss, unexpected costs, or market changes?
- What tax-planning questions or deadlines should I investigate?
- Which actions could improve my long-term financial position, and what assumptions support them?

The product must earn trust through transparent calculations, evidence-linked explanations, privacy, user control, and clearly communicated uncertainty—not through confident-sounding AI alone.

## Product principles

1. **Decision support over dashboards.** Every major insight should connect evidence to a practical next step.
2. **Correctness before autonomy.** Deterministic financial calculations and explicit rules govern money-related decisions. AI explains and assists; it does not become the source of truth.
3. **User control.** No transfer, trade, cancellation, filing, or other consequential action without appropriate authorization and confirmation.
4. **Privacy and security by design.** Minimize collection, restrict access, encrypt sensitive data, and make consent and revocation understandable.
5. **Global by architecture, local by implementation.** Support localization, currencies, time zones, regional data controls, and country-specific provider/compliance adapters. Do not assume one country’s tax or banking rules apply everywhere.
6. **Explainability.** Show sources, calculations, assumptions, freshness, confidence, and limitations for consequential recommendations.
7. **Accessible and inclusive.** Design for varied levels of financial literacy, device capability, language, income stability, and connectivity.
8. **Reliability over premature complexity.** Introduce distributed infrastructure only when measured product and operational needs justify it.
9. **Open interfaces.** Keep financial data providers, identity vendors, AI models, and cloud-specific services behind replaceable interfaces where practical.
10. **No fabricated certainty.** Forecasts are scenarios, not promises; risk scores must be documented and validated before use.

## Planned capability areas

- **Financial data hub:** connect supported institutions and import statements; normalize accounts, balances, transactions, assets, and liabilities.
- **Spending intelligence:** categorization, recurring payments, unusual changes, subscriptions, and behavioral trends.
- **Cash-flow forecasting:** upcoming income and obligations, low-balance warnings, scenario analysis, and forecast uncertainty.
- **Financial resilience:** emergency-fund coverage, debt burden, liquidity, concentration, and exposure to income shocks.
- **Goal planning:** savings targets, debt-paydown scenarios, major purchases, and financial-independence estimates.
- **Investment insights:** portfolio visibility, diversification and fee analysis, risk education, and goal-aligned scenario modelling.
- **Tax planning support:** jurisdiction-aware checklists, estimates, deadline reminders, and opportunities to discuss with a qualified professional.
- **AI financial copilot:** grounded explanations, comparisons, summaries, and action plans using authorized user data.
- **Consent and governance:** permissions, data export/deletion workflows, audit trails, and transparent model/provider usage.

These are product goals, not statements that all features are available today.

## Target technology direction

The detailed rationale, alternatives, and rollout sequencing live in [the architecture decision document](docs/architecture/README.md).

| Layer | Proposed direction |
|---|---|
| Mobile | React Native + Expo, TypeScript |
| Web | Next.js + TypeScript |
| Public API / BFF | TypeScript, Fastify, Zod, OpenAPI |
| Money-critical services | Kotlin + Spring Boot when service boundaries are justified |
| Financial system of record | PostgreSQL; explicit double-entry journal if/when money movement or accounting-grade postings are in scope |
| Cache | Redis-compatible managed cache, only where needed |
| Durable workflows | Temporal or managed workflow/queue services, selected after workload evaluation |
| Eventing | Begin with managed queues and transactional outbox; introduce Kafka when scale and replay needs justify it |
| Analytics | Object storage + Parquet/Iceberg and a warehouse/OLAP engine when required |
| ML / forecasting | Python, FastAPI, evaluation and model-versioning pipeline |
| AI | Provider abstraction; structured outputs, constrained tools, grounding, evaluation, and policy gates |
| Cloud | AWS as the initial reference platform, with region/cell isolation designed around actual residency and availability requirements |
| Infrastructure | Terraform; managed services first; Kubernetes/EKS only when operationally justified |
| Observability | OpenTelemetry with a managed backend |
| CI/CD | GitHub Actions, automated tests, dependency/security checks, staged deployment |

This is a target architecture, not a mandate to deploy every component on day one. See [the delivery roadmap](docs/roadmap/README.md).

## Architecture and engineering documentation

- [Architecture overview and target stack](docs/architecture/README.md)
- [Security, privacy, and AI safety](docs/security/README.md)
- [Delivery roadmap and release gates](docs/roadmap/README.md)
- [Architecture decision records](docs/decisions/README.md)
- [Contribution guide](CONTRIBUTING.md)

## Non-goals for the initial release

- Building a bank or holding customer deposits.
- Executing trades, transfers, or tax filings autonomously.
- Providing unqualified, universal tax/legal/investment advice.
- Claiming global bank coverage before provider availability is validated by country.
- Training models on identifiable financial data without a clear lawful basis, disclosed purpose, and appropriate consent/controls.
- Deploying a complex microservice fleet before domain boundaries and scale justify it.

## Trust and safety baseline

- Financial amounts use decimal-safe arithmetic and explicit currency codes; never binary floating-point for authoritative monetary calculations.
- All financial records are scoped to a user/tenant and protected by server-side authorization.
- External provider data is treated as untrusted input and validated before use.
- AI output is not authoritative financial state. Tool calls use narrow schemas and least-privilege permissions.
- Consequential actions require deterministic policy checks, idempotency, audit records, and appropriate user confirmation.
- Forecasts include assumptions, ranges where appropriate, and timestamps for source-data freshness.
- Secrets are never committed to source control. Use managed secret storage and rotate exposed credentials.
- Security, privacy, regulatory, and accessibility requirements must be assessed per launch jurisdiction.

## Development status

The first milestone is to establish a reliable foundation: product requirements, architecture boundaries, data contracts, security baseline, CI checks, and a thin end-to-end vertical slice. Implemented functionality should be documented only after it exists and passes the relevant tests.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). All contributions should preserve financial correctness, user privacy, clear documentation, and testability.

## License

No license has been declared yet. Until a license is added, assume all rights are reserved; do not assume the code is available for reuse under an open-source license.
