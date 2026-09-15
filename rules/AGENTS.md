---
trigger: model_decision
description: "Activate when discussing architecture, authoring, or reviewing Technical Design Docs"
---

# Architecture Governance and Design Docs

When operating within software architecture and Technical Design Docs, strictly follow the corporate guidelines below:

## 1. Applicability Criteria

Design Docs are mandatory for projects with:

- Cross-squad impact and dependencies across multiple microservices.
- Breaking changes to API contracts or events.
- New cloud components (databases, queues) or vendor/SaaS adoption.
- Processing and storage of sensitive or personal data.

## 2. C4 Container Modeling (Level 2)

- **Mandatory**: Every design doc must contain a C4 Container (Level 2) diagram.
- **Pure and Technology-Agnostic**: C4 nodes represent software topology and runtime execution. It is strictly forbidden to embed Git repository URLs, squad names, or physical assignees inside C4 nodes.
- **Mandatory Expansion**: Systems whose internal code/architecture will be modified must be expanded using `subgraph` blocks. Peripheral partner/external systems are tagged as `[External System]`.
- **Node Formatting**: `[Person]`, `[Software System]`, `[External System]`, `[Container: Technology]`.
- **Relationship Protocols**: Every relationship arrow must specify the action and protocol/format: `Origin -->|"Action (Protocol: REST, gRPC, Kafka, etc)"| Destination`.

## 3. Sequence Diagrams (`sequenceDiagram`)

- `autonumber` is mandatory.
- Must cover happy paths and exception handling using conditional blocks (`alt / else` and `opt`).
- Clear semantic qualification (human actors as `actor`, systems/components as `participant`).

## 4. Security, Privacy, and Data Protection (GDPR, LGPD, etc.)

- If financial flows or high-risk transactions are present, the architecture must include an explicit AppSec review.
- If storing or processing new personal data, the Data Governance / Privacy Team (DPO) must be engaged in compliance with applicable regulatory frameworks (such as GDPR, LGPD).

## 5. Validations, Pre-commit, and Commits (Husky)

- **Local Quality Integration:** This repository uses **Husky + lint-staged** with YAML formatting checks (`prettier`), JSON Schema validation (`ajv-cli`), and Markdown linting (`markdownlint-cli2`).
- **Markdown Lint:** `markdownlint-cli2` runs on all `.md` files. Global rule overrides and directory ignores reside in `.markdownlint-cli2.jsonc` at the root. *(Note: The `templates/` folder may retain a local `.markdownlint.json` for template-specific rules).*
- **Configuration Changes:** Whenever modifying `templates/config.yaml`, verify that the structure strictly conforms to `templates/config.schema.json`. Husky hooks (via lint-staged) validate these changes and will block commits if invalid.
- **Prettier:** Automated JS-based formatting ensuring YAML consistency without requiring Python dependencies.

## 6. Conventional Commits and Releases

- **Commitlint:** All commits must strictly follow the Angular / Conventional Commits standard. The `.husky/commit-msg` hook blocks non-compliant messages. Example: `feat: add new c4 diagram feature`, `fix: correct markdownlint rules`.
- **Release:** To publish a new official version (with automated changelog and Git tag), manually run `npm run release`. **Never modify `plugin.json` or `package.json` manually to bump versions**. `release-it` manages version synchronization automatically based on commit history.

## 7. Specs and ADR Lifecycle

- **Specs as ADRs:** Technical specification and design documents produced during the planning phase must not be committed as standalone loose specs. They must be structured and committed directly as an *Architecture Decision Record* (ADR) during the planning step itself.
