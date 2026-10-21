# Architecture Decision Records (ADRs)

ADRs record important decisions, their context, and when they should be revisited. A decision is not permanent truth: revisit it when product scope, jurisdiction, team capability, costs, or measured workloads change.

## Status vocabulary

- **Proposed:** recommended, not yet validated in implementation.
- **Accepted:** approved for the current scope.
- **Superseded:** replaced by a later decision.
- **Deprecated:** no longer recommended for new work.

The entries below are proposed starting decisions for an early-stage project.

## ADR-001 — Build a modular monolith before microservices

**Status:** Proposed

**Context:** SPFS Pro needs strong domain boundaries but is at an early stage. Distributed systems add deployment, tracing, networking, consistency, and on-call costs.

**Decision:** Start with a modular monolith and separate background workers where useful. Define domain interfaces and data ownership so modules can be extracted later. Split a module into a service only for a demonstrated scaling, security, team ownership, availability, or deployment reason.

**Consequences:** Faster iteration and simpler local development. Requires discipline to prevent cross-module database coupling.

**Revisit when:** independent scaling, isolation, deployment cadence, or ownership boundaries are measurably constrained.

## ADR-002 — TypeScript for product surfaces and initial API

**Status:** Proposed

**Context:** Shared types and rapid iteration matter for web, mobile, and the initial API.

**Decision:** Use TypeScript for React Native/Expo, Next.js, and the initial Fastify API/BFF. Use runtime schema validation and OpenAPI contracts; compile-time types alone do not validate untrusted network input.

**Consequences:** Shared tooling and strong developer velocity. Avoid turning shared packages into a tightly coupled distributed monolith.

**Revisit when:** a workload or team boundary clearly benefits from another runtime.

## ADR-003 — Kotlin for future money-critical service boundaries

**Status:** Proposed

**Context:** A dedicated JVM service may be valuable for high-integrity financial workflows as the product matures.

**Decision:** Kotlin + Spring Boot is the preferred candidate for separately deployed money-critical services if service extraction is justified. It is not a prerequisite for the first release.

**Consequences:** Strong JVM ecosystem and explicit service ownership, with additional language/tooling overhead if adopted too early.

**Revisit when:** the ledger, payments, or another high-integrity domain requires independent ownership, deployment, or operational isolation.

## ADR-004 — PostgreSQL as the initial system of record

**Status:** Proposed

**Context:** The early product needs transactions, relational integrity, migrations, and robust query support.

**Decision:** Use managed PostgreSQL for authoritative application state. Keep ownership explicit and avoid a shared database as an excuse for unrestricted cross-domain writes. Add cache only for demonstrated hot paths.

**Consequences:** Familiar operational model and strong transactional guarantees. Requires indexing, backup/restore drills, migration discipline, and tenant-isolation testing.

**Revisit when:** a measured workload or availability/residency requirement exceeds the selected managed deployment's capabilities.

## ADR-005 — Financial calculations are deterministic and currency-aware

**Status:** Proposed

**Context:** Incorrect arithmetic can cause direct user harm.

**Decision:** Use integer minor units or decimal arithmetic with explicit currency and scale; never use binary floating point as the authoritative representation of money. Record exchange-rate source and timestamp for conversions. Separate display rounding from calculation rules and test invariants.

**Consequences:** More explicit data contracts and tests, substantially lower risk of silent rounding/currency errors.

**Revisit when:** not applicable as a principle; implementation details may evolve without relaxing correctness requirements.

## ADR-006 — AI advisory plane is separate from the execution plane

**Status:** Proposed

**Context:** LLMs can produce plausible but incorrect content and may be manipulated by untrusted text.

**Decision:** Models may explain, summarize, compare, and propose. Deterministic application services own financial calculations, authorization, policy enforcement, and consequential actions. Default AI tools to read-only; high-impact actions require separate controls and user confirmation.

**Consequences:** Slightly more orchestration, with better safety, auditability, and testability.

**Revisit when:** never remove the separation. Individual tool permissions may be expanded only after a documented risk review and required approvals.

## ADR-007 — Provider adapters prevent integration lock-in

**Status:** Proposed

**Context:** Open-banking, brokerage, payroll, tax, and identity coverage differs by country and provider.

**Decision:** Define internal provider-neutral contracts and isolate vendor SDKs, identifiers, errors, cursors, and webhooks in adapters. Preserve source provenance and provider-specific details where needed for reconciliation.

**Consequences:** Some up-front abstraction cost; easier provider replacement and market expansion.

**Revisit when:** refine contracts based on real provider behavior, but do not leak vendor assumptions throughout core domains.

## ADR-008 — Managed cloud services first; AWS as reference platform

**Status:** Proposed

**Context:** Global ambitions require a credible deployment path, but early-stage teams must minimize undifferentiated operations.

**Decision:** Use AWS as the initial reference cloud and prefer managed services. Adopt Kubernetes/EKS, Kafka, multi-region active-active, and cell-based deployment only when requirements and team maturity justify them. Design data boundaries so future regional cells can be isolated.

**Consequences:** Lower initial operational burden and a clear regional path. Some cloud-specific integration is expected; abstract business logic and provider contracts, not every infrastructure primitive.

**Revisit when:** launch market, residency rules, availability objectives, or cost measurements indicate another design is better.

## ADR-009 — Queue and transactional outbox before a Kafka platform

**Status:** Proposed

**Context:** Background work and reliable events are needed, but Kafka adds operational and schema-governance costs.

**Decision:** Begin with a transactional outbox and managed queue/worker model. Adopt Kafka when replay, fan-out, ordering, throughput, and consumer needs justify it.

**Consequences:** Simpler initial operations; future event contracts must still be versioned and consumers idempotent.

**Revisit when:** measured event workloads and operational needs justify streaming infrastructure.

## ADR-010 — Global adoption through explicit market capability gates

**Status:** Proposed

**Context:** A product cannot safely assume identical financial data access, tax law, advice regulation, or residency obligations in every country.

**Decision:** Maintain a market capability registry and launch one market only after provider, legal, privacy, security, localization, and support readiness reviews. Universal calculations must be separated from jurisdiction-specific rules.

**Consequences:** Market expansion is deliberate rather than instant, and unavailable features can be clearly communicated.

**Revisit when:** each market's capability and risk profile changes; no blanket global-compliance assumption is permitted.

## Decision template

For future decisions, create a file at docs/decisions/NNNN-short-title.md with:

1. Title and status
2. Date and owner
3. Context and constraints
4. Options considered
5. Decision and rationale
6. Consequences and risks
7. Security/privacy implications
8. Conditions for revisiting
