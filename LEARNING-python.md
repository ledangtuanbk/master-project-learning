# Project Learning Checklist

A step-by-step guide to mastering this Python project, going from a bird's-eye view down to implementation details. Run the prompts in order — each one builds on the last. Save every response to `docs-local/learning/` so you build a permanent knowledge base you can revisit.

## How to use this file

1. Open a Claude Code session in the project root (or paste prompts into the Claude web/desktop app with the project shared).
2. Run prompts **one at a time**, in order. Don't batch them — each answer should inform the next question.
3. Each prompt ends with a "Save to" instruction. Claude will write the response directly into `docs-local/learning/`.
4. After Phase 2, try to *predict* the answer before asking. That's where real understanding sticks.
5. In parallel, get the project running locally and pick one small bug or feature to ship. Reading explains the code; changing it teaches you the code.

Tick each box as you go:

- [ ] 01 — Elevator pitch
- [ ] 02 — Directory map
- [ ] 03 — Entry points
- [ ] 04 — Architecture diagram
- [ ] 05 — Layering & conventions
- [ ] 06 — Domain model
- [ ] 07 — Request trace
- [ ] 08 — Persistence
- [ ] 09 — Async & background jobs
- [ ] 10 — Configuration
- [ ] 11 — Auth, errors, logging
- [ ] 12 — Testing
- [ ] 12b — Tooling & quality gates
- [ ] 12c — Dependencies & environment
- [ ] 13 — Complex areas
- [ ] 14 — Key libraries
- [ ] 15 — NEWCOMER.md
- [ ] 16 — Index page

---

## The save-instruction suffix

Every prompt below already includes its save target. The general format Claude follows is:

```
# <Title>

> **Prompt:** <one-line summary of what I asked>
> **Date:** <today's date>

<full answer with proper markdown — headings, fenced code blocks
with language tags, Mermaid diagrams where useful, file paths as
`backticks`, line refs as `path/to/file.py:42`>

## Open questions
<anything Claude wasn't sure about — come back to these>
```

---

## Phase 1: Orient yourself

### Prompt 01 — Elevator pitch

> Read the README, pyproject.toml (or setup.py / setup.cfg / requirements.txt), and top-level directory structure. In plain language, tell me: what does this project do, who is it for, what problem does it solve, and what's the tech stack (framework, Python version, key libraries)? Give me a 5-sentence summary I could repeat to someone else.
>
> Save your full response to `docs-local/learning/01-overview.md` using proper markdown (headings, code blocks with language tags, Mermaid where useful). Include the prompt and today's date at the top, and an "Open questions" section at the end.

### Prompt 02 — Map the territory

> Generate a directory tree (2–3 levels deep) and annotate each folder/package with one sentence explaining its purpose. Note which folders are Python packages (have `__init__.py`) vs plain directories. Flag any folders whose role isn't obvious from the name.
>
> Save your full response to `docs-local/learning/02-directory-map.md` with the standard format (prompt + date at top, open questions at bottom).

### Prompt 03 — Entry points

> Where does execution start? List every entry point: `if __name__ == '__main__'` blocks, `console_scripts` in pyproject.toml, FastAPI/Flask/Django app objects, Celery workers, Click/Typer/argparse CLIs, scheduled tasks, and exposed HTTP routes. For each, give the file path and what it kicks off.
>
> Save your full response to `docs-local/learning/03-entry-points.md` with the standard format.

---

## Phase 2: Architecture

### Prompt 04 — The big picture

> Draw an architecture diagram in Mermaid showing the major modules/packages, how they communicate, what external systems they depend on (Postgres, Redis, S3, third-party APIs), and the direction of data flow.
>
> Save your full response to `docs-local/learning/04-architecture.md` with the standard format. Embed the Mermaid diagram in a fenced ```mermaid block.

### Prompt 05 — Layering & conventions

> What architectural pattern does this project follow (MVC à la Django, layered, hexagonal/clean, service-repository, microservices, etc.)? Show me a concrete example by tracing one feature from the outer layer (view/router) down through services to the data layer.
>
> Save your full response to `docs-local/learning/05-layering.md` with the standard format.

### Prompt 06 — Domain model

> What are the core domain entities/models? List them with their key fields and relationships. Distinguish between ORM models (SQLAlchemy/Django ORM), Pydantic schemas/dataclasses, and any pure domain objects. Explain how they map to each other.
>
> Save your full response to `docs-local/learning/06-domain-model.md` with the standard format. Use Mermaid `erDiagram` or `classDiagram` for the relationships.

---

## Phase 3: Flows

### Prompt 07 — Trace a request end-to-end

> Pick the most important user-facing feature. Walk me through what happens from the moment a request enters the system (route handler) to the response going back out — every function, every layer, every side effect, including middleware and decorators. Include file paths and line numbers.
>
> Save your full response to `docs-local/learning/07-request-trace.md` with the standard format. Use a Mermaid `sequenceDiagram` for the flow.

### Prompt 08 — State & persistence

> How does data flow into and out of storage? Show me the schema (models + migrations), the ORM/query layer, session/transaction management, connection pooling, and where caching sits. Note any raw SQL.
>
> Save your full response to `docs-local/learning/08-persistence.md` with the standard format.

### Prompt 09 — Async & background work

> Identify all asynchronous code: `async def` functions and the event loop setup, Celery/RQ/Dramatiq tasks, APScheduler jobs, message consumers, websockets. For each, explain what triggers it, what it produces, and whether sync and async code mix anywhere risky.
>
> Save your full response to `docs-local/learning/09-async-and-jobs.md` with the standard format.

---

## Phase 4: Cross-cutting concerns

### Prompt 10 — Configuration & environments

> How is the app configured? List every environment variable, settings file (settings.py, config.py, Pydantic Settings, .env), and feature flag. Note which are required vs optional, defaults, and what each controls. Identify how secrets are handled.
>
> Save your full response to `docs-local/learning/10-configuration.md` with the standard format. Present env vars in a table with columns: Name / Required / Default / Purpose.

### Prompt 11 — Auth, security, error handling, logging

> Walk me through authentication and authorization (JWT, sessions, OAuth, decorators/dependencies). Then show me the error-handling strategy — custom exceptions, exception handlers, the logging setup (logging config, structlog, log levels).
>
> Save your full response to `docs-local/learning/11-auth-and-errors.md` with the standard format.

### Prompt 12 — Testing

> What's the test setup? Show me the pytest configuration, fixtures, `conftest.py` hierarchy, the unit/integration/e2e split, how to run tests, mocking conventions (mock, monkeypatch, pytest-mock, responses, vcrpy), and coverage. Flag under-tested areas.
>
> Save your full response to `docs-local/learning/12-testing.md` with the standard format.

### Prompt 12b — Tooling & quality gates

> What tools enforce code quality? Check for ruff/flake8/pylint, black/isort, mypy/pyright, pre-commit hooks, and CI workflows. Show me the configuration for each and what rules are enabled or disabled. Is the codebase fully type-hinted, partially, or not at all?
>
> Save your full response to `docs-local/learning/12b-tooling.md` with the standard format.

### Prompt 12c — Dependencies & environment

> How is the Python environment managed (venv, poetry, uv, pipenv, conda, pip-tools)? How are dependencies locked? List runtime vs dev dependencies and call out anything pinned to an unusually old or new version.
>
> Save your full response to `docs-local/learning/12c-dependencies.md` with the standard format.

---

## Phase 5: Going deep

### Prompt 13 — The tricky parts

> Find the 3–5 most complex or fragile parts of the codebase — heavy metaclass/decorator magic, deep inheritance, monkey-patching, dynamic imports, threading/multiprocessing, places with lots of recent commits or "here be dragons" comments. Explain what each does and why it's complex.
>
> Save your full response to `docs-local/learning/13-complex-areas.md` with the standard format.

### Prompt 14 — Dependencies under the hood

> Pick the 5 most-used third-party libraries. For each, explain why this project uses it, what would break without it, whether it's used idiomatically, and whether the version pin is current.
>
> Save your full response to `docs-local/learning/14-key-libraries.md` with the standard format.

### Prompt 15 — Onboarding doc

> Based on everything you now know, write me a NEWCOMER.md — setup steps (Python version, venv, installing deps, running migrations, starting the dev server), what a new developer should read first, in what order, and what they should build or break to learn the system.
>
> Save your full response to `docs-local/learning/15-NEWCOMER.md`. This one is the deliverable itself — format it as a polished onboarding doc rather than a Q&A response.

### Prompt 16 — Build the index

> Read every file in `docs-local/learning/` and create a table of contents with one-sentence summaries of each document, plus a "Suggested reading order" section for someone new to the project.
>
> Save your full response to `docs-local/learning/README.md`. This is the hub — make it scannable.

---

## Tips for getting better answers

- **Point at files, don't grep blindly.** For Prompts 4–7, mention specific files (`@app/main.py`, `@app/api/routes.py`) instead of asking Claude to scan the whole repo. Sharper context, sharper answers.
- **Run the project before Prompt 7.** Tracing a request makes ten times more sense when you can hit the endpoint and see the response yourself.
- **Don't skip Prompt 16.** A folder of 16 markdown files without an index is just a pile. The README turns it into a map.
- **Revisit after one month.** Re-run Prompts 04, 06, and 13 a month in — your mental model will be different, and the diffs are where the real learning shows up.
