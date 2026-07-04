# ADR-0000: Project Charter

## Status

Accepted

## Context

This project is an Enterprise AI Engineering Learning Platform using FIFA World Cup forecasting as a practical case study. The project must balance education, engineering excellence, and software architecture with a working production-grade application.

## Decision

We establish this project charter as the foundational architecture decision record. All subsequent ADRs derive from and align with this charter.

### Core Principles

1. **Learning First** — every implementation must teach a concept.
2. **Documentation First** — no feature is complete until documented.
3. **API First** — every capability through a well-designed FastAPI endpoint.
4. **Clean Architecture** — business logic never depends on frameworks.
5. **Test Everything** — every feature includes automated tests.
6. **Explain Everything** — every decision includes explanation and trade-off analysis.
7. **Production Mindset** — learning examples follow production-quality practices.
8. **Build for Change** — modules evolve without major rewrites.
9. **Open Source Quality** — every commit improves the repository as a learning resource.
10. **Continuous Improvement** — architecture evolves with new insights.

### Architecture Drivers

- **Extensibility** — support multiple sports and forecasting domains.
- **Learnability** — a developer with basic Python should understand every decision.
- **Testability** — comprehensive automated testing.
- **Maintainability** — Clean Architecture with clear separation of concerns.

### Scope

- **In scope**: Football (soccer) forecasting, multi-sport extension, educational content, production-grade engineering.
- **Out of scope**: Real-time betting, live match streaming, mobile applications.

## Consequences

### Positive

- Clear architectural direction for all contributors.
- Educational value maintained throughout development.
- High engineering quality standards.

### Negative

- Slower initial development due to documentation and testing requirements.
- More complex architecture than a simple prediction script.

## Compliance

All features must align with the Engineering Constitution. Deviations require a new ADR documenting the rationale.
