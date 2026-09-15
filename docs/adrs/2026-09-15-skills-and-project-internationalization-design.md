# ADR: Skills and Project Internationalization for Open-Source Distribution

## 1. Context

The Dede Agent was originally conceived with core prompts, skills, and architectural governance rules authored in Brazilian Portuguese (`pt-BR`). To distribute the agent openly across global developer communities (such as GitHub and the Antigravity ecosystem), having primary documentation, skill definitions, and configuration files in English is required for standard discoverability and adoption.

## 2. Decision

We have decided to standardize the project's core artifacts into English while preserving intelligent multilingual generation capabilities for end-users:

1. **Skills (`skills/*/SKILL.md`)**:
   - Translate all metadata `description` fields and body instructions for `design-doc:create`, `design-doc:export`, and `design-doc:review` into English.
   - Retain smart bilingual document generation: the agent operates with English instructions, but seamlessly creates design docs in either English or Portuguese based on the user's interaction language using `templates/default_en.md` and `templates/default_pt.md`.
   - Standardize review checklist verdicts and headings into English (`FINAL VERDICT: [APPROVED / APPROVED WITH RESERVATIONS / CHANGES REQUIRED]`, `Blocking Issues (Blockers)`, `Improvement Opportunities (Recommendations)`).

2. **Governance Rules (`rules/AGENTS.md`)**:
   - Translate the entire corporate architecture governance into English.
   - Generalize Section 4 to "Security, Privacy, and Data Protection (GDPR, LGPD, etc.)" to serve international compliance standards.

3. **Configuration (`templates/config.yaml`)**:
   - Translate profile names, descriptions, and custom section titles/prompts into English while maintaining strict adherence to `templates/config.schema.json`.

4. **Project Presentation (`package.json`, `README.md`)**:
   - Update `package.json` description to English.
   - Establish English `README.md` as the primary repository presentation, renaming the Portuguese version to `README-pt.md` with cross-links.

## 3. Consequences

- Global developers can easily understand, contribute to, and install the Dede Agent plugin.
- Local Portuguese-speaking teams can continue authoring Portuguese design docs seamlessly via `templates/default_pt.md`.
- Automated pre-commit checks (markdownlint, ajv schema validation, and prettier) remain enforced and verified against the updated files.
