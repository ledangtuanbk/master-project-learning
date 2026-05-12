# Project Learning Checklist — Java + Spring Boot

A step-by-step guide to mastering this Spring Boot project, going from a bird's-eye view down to implementation details. Run the prompts in order — each one builds on the last. Save every response to `docs/learning/` so you build a permanent knowledge base you can revisit.

## How to use this file

1. Open a Claude Code session in the project root (or paste prompts into the Claude web/desktop app with the project shared).
2. Run prompts **one at a time**, in order. Don't batch them — each answer should inform the next question.
3. Each prompt ends with a "Save to" instruction. Claude will write the response directly into `docs/learning/`.
4. After Phase 2, try to *predict* the answer before asking. That's where real understanding sticks.
5. In parallel, get the project running locally (`./mvnw spring-boot:run` or `./gradlew bootRun`) and pick one small bug or feature to ship. Reading explains the code; changing it teaches you the code.

Tick each box as you go:

- [ ] 01 — Elevator pitch
- [ ] 02 — Module & package map
- [ ] 03 — Entry points & beans
- [ ] 04 — Architecture diagram
- [ ] 05 — Layering & conventions
- [ ] 06 — Domain model
- [ ] 07 — Request trace
- [ ] 08 — Persistence
- [ ] 09 — Async, scheduling & messaging
- [ ] 10 — Configuration & profiles
- [ ] 11 — Security, errors, logging
- [ ] 12 — Testing
- [ ] 12b — Build & quality gates
- [ ] 12c — Dependencies & versions
- [ ] 13 — Complex areas
- [ ] 14 — Key libraries & starters
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
with language tags (```java, ```yaml, ```sql), Mermaid where useful,
file paths as `backticks`, line refs as `path/to/File.java:42`>

## Open questions
<anything Claude wasn't sure about — come back to these>
```

---

## Phase 1: Orient yourself

### Prompt 01 — Elevator pitch

> Read the README, `pom.xml` or `build.gradle(.kts)`, and top-level directory structure. In plain language, tell me: what does this project do, who is it for, what problem does it solve, and what's the tech stack (Java version, Spring Boot version, build tool, key starters and libraries)? Give me a 5-sentence summary I could repeat to someone else.
>
> Save your full response to `docs/learning/01-overview.md` using proper markdown. Include the prompt and today's date at the top, and an "Open questions" section at the end.

### Prompt 02 — Module & package map

> Generate a tree of source modules and packages (down to ~3 levels). For a multi-module Maven/Gradle project, list each module and its role. Within each module, annotate the top packages (`controller`, `service`, `repository`, `domain`, `config`, `dto`, etc.) with one sentence on purpose. Flag anything whose role isn't obvious from the name.
>
> Save your full response to `docs/learning/02-module-map.md` with the standard format.

### Prompt 03 — Entry points & beans

> Where does execution start? Locate the `@SpringBootApplication` class and list every entry point: REST controllers (`@RestController`, `@Controller`), `CommandLineRunner` / `ApplicationRunner` beans, `@Scheduled` methods, `@KafkaListener` / `@RabbitListener` / `@JmsListener`, `@EventListener`, WebSocket endpoints, and any `main` methods beyond the boot class. For each, give the file path and what it kicks off. Also note any `@ComponentScan` customizations or `@Import` of external configs.
>
> Save your full response to `docs/learning/03-entry-points.md` with the standard format.

---

## Phase 2: Architecture

### Prompt 04 — The big picture

> Draw an architecture diagram in Mermaid showing the major modules/packages, how they communicate, what external systems they depend on (Postgres/MySQL, Redis, Kafka/RabbitMQ, S3, third-party REST/SOAP APIs), and the direction of data flow. Distinguish synchronous calls from async messaging.
>
> Save your full response to `docs/learning/04-architecture.md` with the standard format. Embed the Mermaid diagram in a fenced ```mermaid block.

### Prompt 05 — Layering & conventions

> What architectural pattern does this project follow (classic layered Controller→Service→Repository, hexagonal/ports-and-adapters, DDD, CQRS, modular monolith, microservice, etc.)? Show me a concrete example by tracing one feature from controller down through service to repository, including DTO ↔ entity mapping (MapStruct, ModelMapper, or manual). Note any cross-cutting use of AOP (`@Aspect`, `@Transactional`, custom annotations).
>
> Save your full response to `docs/learning/05-layering.md` with the standard format.

### Prompt 06 — Domain model

> What are the core domain entities? List them with key fields and relationships. Distinguish between JPA entities (`@Entity`), DTOs/request-response records, and any pure domain objects or value objects. Show JPA relationship annotations (`@OneToMany`, `@ManyToOne`, fetch types, cascade settings) and how DTOs map to entities.
>
> Save your full response to `docs/learning/06-domain-model.md` with the standard format. Use Mermaid `erDiagram` for entity relationships and `classDiagram` if helpful for the DTO/entity split.

---

## Phase 3: Flows

### Prompt 07 — Trace a request end-to-end

> Pick the most important user-facing endpoint. Walk me through what happens from the moment an HTTP request enters the system to the response going back out — every filter, interceptor, controller method, validation, service call, transaction boundary, repository call, mapper, and exception handler. Include file paths and line numbers, and note where Spring AOP proxies are involved (e.g. `@Transactional` self-invocation pitfalls).
>
> Save your full response to `docs/learning/07-request-trace.md` with the standard format. Use a Mermaid `sequenceDiagram` for the flow.

### Prompt 08 — Persistence

> How does data flow into and out of storage? Show me: the JPA/Hibernate setup, entity scanning, Spring Data repositories (custom queries with `@Query`, derived methods, specifications), transaction management (`@Transactional` placement, propagation, isolation), connection pool config (HikariCP settings), Flyway/Liquibase migrations, and any second-level cache. Note any native SQL or JdbcTemplate usage.
>
> Save your full response to `docs/learning/08-persistence.md` with the standard format.

### Prompt 09 — Async, scheduling & messaging

> Identify all asynchronous code: `@Async` methods and the `TaskExecutor` config, `@Scheduled` jobs, Kafka/RabbitMQ/JMS listeners and producers, `ApplicationEventPublisher` usage, WebFlux reactive code (`Mono`/`Flux`), and `CompletableFuture` chains. For each, explain what triggers it, what it produces, the thread pool it runs on, and any error/retry handling.
>
> Save your full response to `docs/learning/09-async-and-jobs.md` with the standard format.

---

## Phase 4: Cross-cutting concerns

### Prompt 10 — Configuration & profiles

> How is the app configured? List every `application.yml` / `application.properties` file and what each profile (`dev`, `prod`, `test`, etc.) overrides. Catalog `@ConfigurationProperties` classes, `@Value` injections, and feature flags. Note which properties are required vs optional, defaults, and how secrets are sourced (Vault, AWS SM, env vars, Config Server).
>
> Save your full response to `docs/learning/10-configuration.md` with the standard format. Present key properties in a table with columns: Property / Required / Default / Profile(s) / Purpose.

### Prompt 11 — Security, errors, logging

> Walk me through Spring Security: the `SecurityFilterChain` config, authentication mechanism (JWT, OAuth2/OIDC, session, basic), authorization (`@PreAuthorize`, method security, role hierarchy), CORS, CSRF. Then show me the error-handling strategy — `@ControllerAdvice` / `@ExceptionHandler` classes, custom exceptions, the error response format. Finally, the logging setup — Logback/Log4j2 config, log levels per package, structured logging, MDC usage, correlation/trace IDs (Sleuth/Micrometer).
>
> Save your full response to `docs/learning/11-security-and-errors.md` with the standard format.

### Prompt 12 — Testing

> What's the test setup? Cover: unit tests (Mockito, AssertJ), slice tests (`@WebMvcTest`, `@DataJpaTest`, `@JsonTest`), full integration tests (`@SpringBootTest`), Testcontainers usage, MockMvc vs WebTestClient, test profiles, and any contract tests (Spring Cloud Contract, Pact). Show how to run tests and where coverage stands. Flag under-tested areas.
>
> Save your full response to `docs/learning/12-testing.md` with the standard format.

### Prompt 12b — Build & quality gates

> What enforces code quality? Check for Checkstyle, SpotBugs, PMD, ErrorProne, SonarQube config, Spotless/google-java-format, pre-commit hooks, and CI workflows (GitHub Actions, GitLab CI, Jenkins). Show the configuration for each. Also describe the build itself — Maven/Gradle plugins, custom tasks, profiles, and how the Docker image is built (Spring Boot Buildpacks, Jib, or a Dockerfile).
>
> Save your full response to `docs/learning/12b-build-and-quality.md` with the standard format.

### Prompt 12c — Dependencies & versions

> List the Spring Boot version, Java version, and all direct dependencies grouped by purpose (web, data, security, messaging, observability, testing). Note any BOM imports (`spring-boot-dependencies`, `spring-cloud-dependencies`), version pins that override the BOM, and anything stuck on an unusually old version. Flag deprecated starters.
>
> Save your full response to `docs/learning/12c-dependencies.md` with the standard format.

---

## Phase 5: Going deep

### Prompt 13 — The tricky parts

> Find the 3–5 most complex or fragile parts of the codebase — heavy use of reflection or AOP, custom `BeanPostProcessor` / `BeanFactoryPostProcessor`, custom auto-configuration, conditional beans (`@Conditional*`), generics gymnastics, native queries, places where `@Transactional` is easy to misuse (self-invocation, propagation), thread-safety hotspots, or files with lots of recent commits / "here be dragons" comments. Explain what each does and why it's complex.
>
> Save your full response to `docs/learning/13-complex-areas.md` with the standard format.

### Prompt 14 — Key libraries & starters

> Pick the 5–7 most-used third-party libraries and Spring starters. For each, explain why this project uses it, what would break without it, whether it's used idiomatically, and whether the version is current vs the latest stable. Pay extra attention to non-Spring libraries (e.g. Resilience4j, MapStruct, Lombok, Jackson customizations).
>
> Save your full response to `docs/learning/14-key-libraries.md` with the standard format.

### Prompt 15 — Onboarding doc

> Based on everything you now know, write me a NEWCOMER.md — setup steps (JDK version via SDKMAN/asdf, IDE setup, running the app locally with the right profile, required services via Docker Compose, running migrations, seed data), what a new developer should read first, in what order, and what they should build or break to learn the system.
>
> Save your full response to `docs/learning/15-NEWCOMER.md`. This one is the deliverable itself — format it as a polished onboarding doc rather than a Q&A response.

### Prompt 16 — Build the index

> Read every file in `docs/learning/` and create a table of contents with one-sentence summaries of each document, plus a "Suggested reading order" section for someone new to the project.
>
> Save your full response to `docs/learning/README.md`. This is the hub — make it scannable.

---

## Tips for getting better answers

- **Point at files, don't grep blindly.** For Prompts 4–7, mention specific files (`@src/main/java/.../Application.java`, `@.../WebSecurityConfig.java`) instead of asking Claude to scan the whole repo. Sharper context, sharper answers.
- **Run the project before Prompt 7.** Tracing a request makes ten times more sense when you can hit the endpoint with `curl` or Postman and see the response yourself.
- **Watch for AOP surprises.** `@Transactional`, `@Async`, `@Cacheable`, and custom aspects all create proxies — a method calling another method in the same class often bypasses the proxy. Ask Claude to flag these in Prompt 13.
- **Don't skip Prompt 16.** A folder of 16 markdown files without an index is just a pile. The README turns it into a map.
- **Revisit after one month.** Re-run Prompts 04, 06, and 13 a month in — your mental model will be different, and the diffs are where the real learning shows up.
