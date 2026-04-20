# Developer Style Guide

## General Principles
- Write code for the next developer, not just for the machine.
- Prefer clarity over cleverness; readable code is maintainable code.
- Follow the SOLID principles: single responsibility, open/closed, Liskov substitution, interface segregation, dependency inversion.
- Keep functions and methods small and focused — each should do one thing well.
- Fail fast: validate inputs at the boundary and surface errors early with meaningful messages.

## Naming & Structure
- Use descriptive, intention-revealing names for variables, functions, classes, and modules.
- Avoid abbreviations unless they are universally understood in the domain (e.g. `id`, `url`).
- Group related logic together; keep unrelated concerns in separate files/modules.
- Prefer flat directory structures; nest only when it adds clarity.

## Code Quality
- Never leave dead code, commented-out blocks, or TODO comments in production code — track them as issues instead.
- Every non-trivial function must have at least one unit test covering the happy path and one covering the primary failure case.
- All public APIs (functions, classes, endpoints) must have concise documentation comments.
- Treat warnings as errors — a clean build is a non-negotiable baseline.

## Error Handling
- Always handle errors explicitly; never swallow exceptions silently.
- Return structured error types rather than plain strings or generic `Error` objects.
- Log errors at the point of origin with enough context to reproduce the problem.
- Distinguish between recoverable errors (retry/fallback) and unrecoverable ones (fail loudly).

## Security & Data
- Never hard-code secrets, credentials, or environment-specific configuration.
- Sanitize and validate all external input before use.
- Apply the principle of least privilege to service accounts, database roles, and API scopes.
- Protect sensitive data in logs, responses, and storage — mask or omit PII where possible.

## Performance & Scalability
- Avoid premature optimization; profile before you tune.
- Use pagination, streaming, or lazy loading for large data sets.
- Cache expensive computations at the appropriate layer; always define a cache invalidation strategy.
- Design for horizontal scalability by default — avoid shared mutable state.

## Review & Collaboration
- Every change must go through a pull request with at least one peer review.
- Pull requests should be small and focused on a single concern.
- Write commit messages in the imperative mood and reference the related issue (e.g. `Fix login timeout — closes #42`).
- Keep the main branch always deployable; use feature flags for incomplete work.
