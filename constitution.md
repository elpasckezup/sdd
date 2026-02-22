# Backend Constitution

## Core Principles

### I. Hexagonal Architecture (NON-NEGOTIABLE)
- The backend MUST follow Hexagonal (Ports & Adapters) architecture.
- Business logic lives in the domain/application core and MUST NOT depend on infrastructure, frameworks, or delivery mechanisms.
- Dependencies MUST point inward:
	- Domain: pure business rules.
	- Application: use-cases/services orchestrating domain.
	- Ports: interfaces defined in the core (incoming and outgoing).
	- Adapters: implementations at the edges (REST controllers, persistence, messaging, observability exporters, etc.).
- Incoming adapters (e.g., REST) call inbound ports; outgoing adapters (e.g., JDBC repositories) implement outbound ports.
- Framework code (Spring) is an adapter concern; core code remains framework-agnostic.

### II. Technology Baseline (NON-NEGOTIABLE)
- Language MUST be Java 22.
- Build and dependency management MUST be Maven.
- Application framework MUST be Spring Boot 4.0.2.
- Any deviation from this baseline is forbidden unless the constitution is formally amended.

### III. API Design & Versioning (NON-NEGOTIABLE when exposing REST)
- REST APIs MAY be built with Spring Web when the use case requires HTTP interfaces.
- API versioning strategy is mandatory for REST:
	- Base path MUST be `/api/v1`.
	- Breaking changes MUST be released under a new major API path (e.g., `/api/v2`).
	- Backwards-compatible changes (additive fields/endpoints) MAY ship within the same major version.
- If REST APIs exist, API documentation MUST be provided using `springdoc-openapi`.
- Controllers (delivery adapters) MUST be thin: translate HTTP ↔ DTOs and call inbound ports; no business logic.

### IV. Persistence & Data Isolation (NON-NEGOTIABLE when persistence is needed)
- Persistence MUST use Spring Data JDBC.
- Repository implementations MUST use JDBC Client for database access.
- Database MUST be PostgreSQL.
- Flyway migrations MUST be used for schema evolution; no runtime schema drift.
- Multi-tenant support is required when a use case demands tenant isolation:
	- PostgreSQL MUST support tenant data isolation via per-tenant schemas and access/credential schemas.
	- A `public` schema MAY hold shared/common data across tenants.
	- A tenant registry entity MUST exist to store at minimum:
		- schema key (tenant identifier → schema mapping)
		- encrypted schema access/credentials required for tenant access
	- Data access MUST ensure that tenant context is applied consistently (no cross-tenant reads/writes).

### V. Observability (NON-NEGOTIABLE)
- The backend MUST include Spring Boot Actuator and Micrometer.
- OpenTelemetry MUST be used to instrument, generate, collect, and export telemetry (metrics, logs, traces).
- Metrics MUST be exportable/scrapable by Prometheus.
- Distributed tracing MUST be exportable to Grafana Tempo.
	- Every API request MUST start a trace or continue the propagated trace context.
- Logs MUST be compatible with Grafana Loki.
	- Logback configuration MUST include appenders for:
		- console
		- file
		- Loki (send logs directly from the application to Loki)

## Backend Constraints & Standards

### Packaging & Boundaries
- Code MUST be organized by architectural layer (core vs adapters) and explicit ports.
- DTOs belong to adapters; domain models belong to core.
- Mapping between DTOs and domain models MUST be explicit.

### Configuration & Secrets
- Credentials and tenant access secrets MUST NOT be committed to the repository.
- Local development MUST rely on environment variables and/or `.env` used by docker compose.

### Database Migration Rules
- All schema changes MUST be represented as Flyway migrations.
- Migrations MUST be deterministic and safe for repeated runs in fresh environments.

## Development Workflow & Local Environment

### Docker Compose (NON-NEGOTIABLE for local dev)
Local development MUST provide a `docker-compose` environment that includes:
- PostgreSQL
- Flyway migrations with scripts to execute
- Grafana (for visualizing telemetry)
- Prometheus
- Grafana Tempo
- Grafana Loki

### Quality Gates
- Core (domain/application) MUST have fast unit tests.
- Adapters (persistence/HTTP) SHOULD have integration tests when feasible, especially for:
	- multi-tenant isolation guarantees
	- Flyway migration correctness
	- repository SQL behavior via JDBC Client

## Governance
This constitution is the source of truth for backend construction in this repository.
- Changes require an explicit amendment in this file with rationale and migration plan.
- PR review MUST verify compliance with:
	- Hexagonal boundaries
	- Java 22 / Maven / Spring Boot 4.0.2 baseline
	- API versioning rules (when REST exists)
	- persistence and multi-tenant isolation rules (when persistence is used)
	- observability requirements

**Version**: 1.0.0 | **Ratified**: 2026-02-11 | **Last Amended**: 2026-02-11
