# Delivery Roadmap

This roadmap describes a gated path from an empty repository to a trusted, useful product. Dates are intentionally omitted until scope, team capacity, launch market, and provider access are confirmed. A phase is complete only when its exit criteria are met.

## Operating principles

- Ship thin vertical slices, not disconnected infrastructure.
- Prefer a modular monolith and managed services until measurements justify more complexity.
- Establish financial correctness, security, and data rights before adding high-impact automation.
- Validate one launch market and a small set of account/provider types before expanding.
- Track assumptions and risks explicitly; do not report planned features as implemented.

## Phase 0 — Product and engineering foundation

### Work
- Define target user, core jobs-to-be-done, initial market, and measurable product outcomes.
- Document supported and excluded use cases, especially the boundaries around regulated advice and money movement.
- Establish repository structure, formatting, linting, type checks, tests, CI, secret scanning, and dependency review.
- Create threat model, data inventory, retention principles, and architecture decision records.
- Define canonical types for Money, Currency, Account, Transaction, BalanceObservation, Forecast, and Insight.
- Build design foundations: accessibility, localization, responsive layouts, and privacy-aware analytics.
- Use synthetic fixtures only.

### Exit criteria
- A new developer can run the documented checks.
- CI blocks broken builds and obvious secret leaks.
- Financial data contracts and rounding/currency policies are reviewed.
- Initial product and architecture risks have named owners.
- No claim is made that a production integration exists without evidence.

## Phase 1 — Trusted read-only MVP

### Work
- Identity and session integration with secure account recovery.
- User consent and connection lifecycle.
- One validated financial-data integration or statement-import workflow for the initial market.
- Raw import tracking, normalization, deduplication, retry-safe sync, and visible freshness status.
- Transaction categorization with user correction and correction history.
- Basic account overview, spending trends, recurring obligations, and a transparent cash-flow view.
- Data export and account/data deletion workflows.
- Read-only AI explanations grounded in computed data; no action tools.

### Exit criteria
- Import replay does not create duplicate transactions.
- Provider outage and revoked consent produce understandable states.
- User-to-user authorization tests pass.
- Monetary calculations pass edge-case and property-based tests.
- Every AI-generated financial claim is grounded in authorized data or clearly labeled as general information.
- Support can diagnose sync failures without exposing unnecessary financial details.

## Phase 2 — Forecasting and financial resilience

### Work
- Forecast expected income, recurring expenses, and projected balances.
- Include uncertainty ranges and explicit assumptions.
- Add emergency-fund coverage, debt-cost prioritization scenarios, liquidity and concentration indicators.
- Create scenario comparisons (for example, income interruption or a major expense).
- Add versioned model evaluations, calibration monitoring, and user feedback loops.

### Exit criteria
- Forecasts are evaluated against time-based holdout data and simple baselines.
- Calibration/error is reported by relevant segment, not only as a global average.
- Users can see source freshness, assumptions, and major exclusions.
- Recommendations do not imply guaranteed outcomes.
- A model can be rolled back or disabled independently of core account access.

## Phase 3 — Goals, investment insights, and tax support

### Work
- Goal-based savings and debt plans with adjustable assumptions.
- Portfolio holdings/fees/diversification analysis where reliable data is available.
- Market-data source, timestamp, currency, and methodology display.
- Tax checklists and estimates limited to a reviewed jurisdiction and tax year.
- Professional-review escalation for complex or high-impact situations.
- Country capability registry and localization expansion.

### Exit criteria
- Investment insights distinguish education and analysis from regulated personalized advice where applicable.
- Tax calculations use reviewed, versioned jurisdiction rules and show assumptions.
- Unsupported jurisdictions are explicitly blocked or labeled unavailable.
- Legal and provider review is complete for each newly launched market.

## Phase 4 — Carefully governed actions

Possible actions may include user-confirmed reminders, savings workflows, or other approved integrations. Transfers, trading, cancellations, and filings are separate high-impact capabilities.

### Required gates
- Specific legal/regulatory and partner approval.
- Deterministic policy engine and explicit authorization.
- Strong confirmation and step-up authentication where appropriate.
- Idempotency, durable workflow state, compensation/reversal, reconciliation, and audit.
- Rate limits, fraud/abuse controls, kill switch, operational runbook, and incident drills.
- Independent security review and tested recovery.

Autonomous execution is not required to prove product value and must not be introduced merely to make the product appear more advanced.

## Phase 5 — Global expansion and cell architecture

### Work
- Add markets through repeatable legal, provider, localization, support, and security gates.
- Separate regional data planes where residency, scale, or blast-radius requirements demand it.
- Automate environment creation and regional deployment.
- Test regional failure, provider degradation, backup restoration, and migration paths.
- Adopt event streaming, independently deployed services, and multi-region patterns only when justified by measured requirements.

### Exit criteria
- Each market has a documented capability matrix and accountable owner.
- Data location and cross-border processing are documented and approved.
- Regional recovery objectives are defined and tested.
- Cells can be deployed and operated independently without hidden shared-state dependencies.

## Cross-cutting engineering requirements

### Every pull request
- Formatting, lint, type checks, unit tests, and relevant integration tests.
- Dependency/secret checks in CI.
- Review of authorization, logging, error handling, and data minimization where relevant.
- Documentation updated for changed contracts, decisions, or operations.

### Every release
- Migration and rollback plan.
- Monitoring, alert thresholds, and a named operational owner.
- Feature flag/kill switch for material new capabilities.
- Release notes and support guidance.
- Security and jurisdiction gates completed for the affected scope.

## Suggested initial repository structure

```text
.
├── apps/
│   ├── mobile/             # React Native + Expo
│   └── web/                # Next.js
├── services/
│   ├── api/                # Initial API/BFF and modular domain layer
│   └── ml/                 # Add when a model-serving boundary is needed
├── packages/
│   ├── contracts/          # Shared API schemas and generated types
│   ├── finance-core/       # Currency-safe deterministic calculations
│   ├── ui/                 # Shared web/mobile primitives where practical
│   └── config/             # Shared lint/TypeScript/test configuration
├── infra/                  # Terraform when infrastructure is introduced
├── docs/
│   ├── architecture/
│   ├── security/
│   ├── roadmap/
│   └── decisions/
└── .github/workflows/
```

This is a proposed layout, not an instruction to create every directory before a working vertical slice exists.
