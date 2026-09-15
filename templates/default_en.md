Template Version: v1.0 | Profile: {{profile_name}}

# [Project Name]
**Status**: [🟡 Draft | 🔵 Under Review | 🟢 Approved]
**Document Version**: `v1.0.0`

### Changelog
| Version | Date | Change | Reason / Reviewer |
|---|---|---|---|
| `v1.0.0` | YYYY-MM-DD | Initial Creation | Base draft submitted. |

---

### 1. Team & Reviewers [REQUIRED]
- **Authors**: [Name - Squad]
- **Reviewers**: [Name - Squad / Architecture]

### 2. Overview & Reference Docs [REQUIRED]
- **Reference Docs**: [PRD / Functional Spec / ADRs / Previous Design Docs]
- Executive technical summary in up to 2 paragraphs.

### 3. Context & Technical Diagnosis [REQUIRED]
- Engineering problem, current stack limitations, and current technical debt.

### 4. Initiative Objectives [REQUIRED]
#### 4.1. Business Objectives
- Broad strategic goals.
#### 4.2. Technical Objectives & SLOs
- Measurable metrics (e.g., p99 < 120ms). Do NOT insert rules or logic here.

### 5. Out of Scope [REQUIRED]
- What will not be technically delivered in this phase.

### 6. Existing Solution
#### 6.1. Current Architecture
- C4 Container Level 2 diagram.
#### 6.2. Current Flows
- Sequence diagram illustrating current bottlenecks.

### 7. Proposed Solution [REQUIRED]
#### 7.1. Objective Solution Rules
- Deterministic domain rules, conditionals, guarantees, and invariants.
#### 7.2. Proposed Architecture
- C4 Container Level 2 (Agnostic to repositories).
#### 7.3. Provisioned Resources
- Table of cloud resources or base infrastructure.
#### 7.4. Proposed Flows
- Sequence diagram with `autonumber` and error handling via `alt/else`.
#### 7.5. Interfaces / Screens [OPTIONAL]
- Screen designs or UX flows.
#### 7.6. Contracts and Payloads
- APIs (REST, gRPC) or event topics signature.

### 8. Implementation Plan & Cross-Repo Rollout [REQUIRED]
- Execution order (Infra, Producers, Consumers, Canary).
- Cross-repo rollback plan.

### 9. Alternative Solutions and Trade-offs [REQUIRED]
- Technical paths evaluated and discarded.

### 10. Testability and Observability [REQUIRED]
- Unit/integration tests.
- OpenTelemetry, RED Metrics, Logs, and Alerts.

### 11. Cross-Squad Impacts and Repository Catalog [REQUIRED]
| Service / Repository | Responsible Squad | Path / URL | Expected Change | Deploy Dependency |
|---|---|---|---|---|
| repo-x | Squad Y | url | Change | Phase Z |

### 12. Security and Privacy [REQUIRED]
- **Identity and Access**: OIDC, RBAC.
- **Data Protection**: Vault, WAF, Encryption.
- **Privacy and Governance**: GDPR.

### 13. Risks and Mitigations Matrix
- Category, Risk, Impact, and Mitigation.

### 14. Open Questions
- Points to be clarified.

### 15. References and Useful Links
- GitHub, Jira, Wikis.
