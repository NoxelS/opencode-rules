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
- Use [Conventional Commits](https://www.conventionalcommits.org/) for all commit messages (e.g. `fix: resolve login timeout — closes #42`, `feat: add user profile page`).
- Keep the main branch always deployable; use feature flags for incomplete work.

## Agent Orchestration & Sub-Agent Delegation
- The main agent **must not** perform explorative, research, file-reading, or information-gathering work directly — always delegate these to sub-agents.
- On receiving any task, immediately decompose it into independent sub-tasks and spawn a dedicated sub-agent for each one before doing any other work.
- Spawn sub-agents aggressively: if a task *could* be parallelised or farmed out, it *must* be. Err on the side of spawning more sub-agents rather than fewer.
- The main agent's sole responsibility is high-level understanding of the overall goal, designing the decomposition strategy, integrating sub-agent results, and deciding the next orchestration step.
- Each sub-agent must be given only the minimal, self-contained context it needs for its specific sub-task — never forward the full conversation history.
- Run all independent sub-agents in parallel; never block on one sub-agent before launching another when they have no dependency between them.
- After every round of sub-agents completes, the main agent consolidates results into a single compact summary before spawning the next wave or producing a final answer.
- Research tasks (reading docs, exploring the codebase, checking external references, analysing logs) must always be handled by sub-agents — the main agent must never perform raw exploration itself.
- Implementation sub-agents should be scoped to a single file, module, or concern; never let one sub-agent own an entire feature end-to-end if it can be split further.
- If a sub-agent's result reveals new unknowns, the main agent must spawn additional sub-agents to resolve them rather than attempting to resolve them inline.
