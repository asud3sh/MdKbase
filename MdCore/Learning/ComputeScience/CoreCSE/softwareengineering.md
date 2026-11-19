
# 15 · SOFTWARE ENGINEERING

## 15.1 Process

### SDLC (Software Development Life Cycle)

**Phases:**
1. Requirements
2. Design
3. Implementation
4. Testing
5. Deployment
6. Maintenance

**Models:**
- **Waterfall:** Sequential; rigid
- **Agile:** Iterative; flexible
- **Spiral:** Risk-driven
- **V-model:** Verification + validation

**Recall:** *Lifecycle of software.*

### Agile

**Manifesto:**
- Individuals and interactions over processes and tools
- Working software over comprehensive documentation
- Customer collaboration over contract negotiation
- Responding to change over following a plan

**Principles:** 12 principles; iterative, incremental, feedback.

**Recall:** *Iterate fast.*

### Scrum

**Roles:** Product Owner, Scrum Master, Team.

**Artifacts:** Product backlog, sprint backlog, increment.

**Ceremonies:** Sprint planning, daily standup, sprint review, retrospective.

**Sprints:** 1–4 weeks.

**Recall:** *Sprint-based Agile.*

### Kanban

**Definition:** Visualize flow; limit WIP.

**Board:** To do, in progress, done.

**WIP limits:** Prevent overload.

**Recall:** *Flow-based Agile.*

### DevOps

**Definition:** Dev + Ops collaboration; culture, automation, measurement, sharing.

**Practices:** CI/CD, IaC, monitoring, blameless postmortems.

**Recall:** *Break dev/ops wall.*

### Requirements Engineering

**Activities:** Elicitation, specification, validation.

**Types:** Functional (what), non-functional (how well).

**Recall:** *What to build.*

---

## 15.2 Design

### System Design

**Goals:** Scalability, availability, consistency, latency.

**Approach:** Back-of-envelope, trade-offs, CAP.

**Components:** Load balancers, caches, databases, queues, CDNs.

**Recall:** *Design at scale.*

### Software Architecture

**Styles:**
- **Layered:** Presentation, business, data
- **Hexagonal:** Ports and adapters
- **Event-driven:** Events as first-class
- **CQRS:** Separate read/write
- **Microservices:** Small services

**Recall:** *Structure of system.*

### API Design

**Styles:**
- **REST:** Resources, HTTP verbs
- **GraphQL:** Query language; client-specified
- **gRPC:** Protobuf; HTTP/2

**Best practices:** Versioning, pagination, error handling, rate limiting.

**Recall:** *Interface design.*

### Design Patterns

**Creational:** Singleton, Factory, Builder, Prototype.
**Structural:** Adapter, Decorator, Facade, Proxy, Composite.
**Behavioral:** Observer, Strategy, Command, State, Iterator.

**Recall:** *Reusable solutions.*

### SOLID Principles

- **S:** Single Responsibility
- **O:** Open/Closed
- **L:** Liskov Substitution
- **I:** Interface Segregation
- **D:** Dependency Inversion

**Recall:** *5 OOP design principles.*

### Domain-Driven Design (DDD)

**Concepts:** Ubiquitous language, bounded contexts, aggregates, entities, value objects, repositories.

**Strategic vs tactical:** Context mapping vs building blocks.

**Recall:** *Model the domain.*

### UML

**Diagrams:** Class, sequence, activity, state, use case, component, deployment.

**Recall:** *Visual modeling language.*

---

## 15.3 Development

### Version Control

**Types:** Centralized (SVN), distributed (Git, Mercurial).

**Git concepts:** Commits, branches, remotes, merges, rebases.

**Workflows:** Git Flow, GitHub Flow, trunk-based.

**Recall:** *Track changes.*

### Git

**Commands:**
- `git init`, `git clone`
- `git add`, `git commit`
- `git branch`, `git checkout`, `git merge`
- `git rebase`, `git reset`, `git revert`
- `git push`, `git pull`, `git fetch`

**Reset vs revert vs rebase:**
- Reset: move HEAD; discard commits
- Revert: new commit undoing changes
- Rebase: replay commits on new base

**Recall:** *Distributed VCS.*

### Code Review

**Practices:** Small PRs, clear descriptions, inline comments, constructive feedback.

**Tools:** GitHub, GitLab, Gerrit.

**Recall:** *Peer review code.*

### Refactoring

**Definition:** Improve structure without changing behavior.

**Techniques:** Extract method, rename, inline, move, extract class.

**Recall:** *Clean without changing behavior.*

### Technical Debt

**Definition:** Shortcuts that cost later.

**Types:** Intentional, unintentional, reckless, prudent.

**Management:** Track, prioritize, repay.

**Recall:** *Borrowed time in code.*

### Documentation

**Types:** README, API docs, ADRs, comments, tutorials.

**Docs-as-code:** Version-controlled, reviewed.

**Recall:** *Write for future readers.*

### Static Analysis

**Tools:** Linters, type checkers, abstract interpretation.

**Examples:** ESLint, Pylint, SonarQube, Coverity.

**Recall:** *Analyze without running.*

### Formal Specification

**Languages:** TLA+, Alloy, Z.

**Use:** Specify behavior; verify properties.

**Recall:** *Prove design correct.*

---

## 15.4 Quality & Operations

### Testing

**Levels:**
- **Unit:** Test smallest unit
- **Integration:** Test components together
- **E2E:** Full system
- **Acceptance:** User requirements

**Test pyramid:** Many unit, fewer integration, few E2E.

**Recall:** *Verify behavior.*

### Unit Testing

**Definition:** Test individual functions/classes.

**Frameworks:** JUnit, pytest, Jest, RSpec.

**Practices:** Arrange-Act-Assert, mocks, coverage.

**Recall:** *Test one thing.*

### Integration Testing

**Definition:** Test interactions between components.

**Approaches:** Big bang, top-down, bottom-up, sandwich.

**Recall:** *Test interactions.*

### E2E Testing

**Definition:** Test full system from user perspective.

**Tools:** Selenium, Cypress, Playwright.

**Recall:** *Test the whole flow.*

### Property-Based Testing

**Definition:** Generate inputs; check invariants.

**Tools:** QuickCheck, Hypothesis, fast-check.

**Recall:** *Test properties, not cases.*

### Fuzzing

**Definition:** Random inputs to find crashes.

**Tools:** AFL, libFuzzer, OSS-Fuzz.

**Recall:** *Throw garbage, find bugs.*

### CI/CD

**CI:** Continuous Integration; merge often, test automatically.

**CD:** Continuous Delivery (deploy-ready) / Deployment (auto-deploy).

**Tools:** GitHub Actions, Jenkins, GitLab CI, CircleCI.

**Recall:** *Automate build → deploy.*

### Build Systems

**Tools:** Make, Bazel, Gradle, Nix, Maven.

**Reproducible builds:** Same inputs → same outputs.

**Recall:** *Automate compilation.*

### Feature Flags

**Definition:** Toggle features without deploy.

**Tools:** LaunchDarkly, Unleash, Flagsmith.

**Recall:** *Toggle without deploy.*

### Observability

**Three pillars:**
- **Logs:** Events
- **Metrics:** Numbers over time
- **Traces:** Request flow

**Tools:** OpenTelemetry, Prometheus, Grafana, Jaeger, Loki.

**Recall:** *Understand system from outside.*

### Monitoring

**Dashboards:** Visualize metrics.

**Alerts:** Notify on thresholds.

**SLOs:** Service Level Objectives.

**Recall:** *Watch system health.*

### Logging

**Levels:** DEBUG, INFO, WARN, ERROR, FATAL.

**Structured:** JSON for parsing.

**Tools:** ELK (Elasticsearch, Logstash, Kibana), Loki.

**Recall:** *Record events.*

### Tracing

**Definition:** Track request across services.

**Concepts:** Trace, span, context propagation.

**Tools:** Jaeger, Zipkin, OpenTelemetry.

**Recall:** *Follow request across services.*

### SRE (Site Reliability Engineering)

**Definition:** Apply software engineering to operations.

**Concepts:** Error budgets, toil reduction, SLOs.

**Recall:** *Ops as engineering.*

### SLOs & SLIs

- **SLI:** Service Level Indicator (metric)
- **SLO:** Service Level Objective (target)
- **SLA:** Service Level Agreement (contract)

**Recall:** *Measure + promise reliability.*

### Incident Response

**Phases:** Detect → triage → mitigate → resolve → postmortem.

**Blameless postmortem:** Focus on systems, not people.

**Recall:** *Handle outages calmly.*

**SOTA:** Platform engineering, GitOps, AI-assisted coding, chaos engineering, developer experience (DevEx), agentic development environments (Google Antigravity with Gemini 3.5 Flash enabling autonomous multi-hour coding sessions).

---
