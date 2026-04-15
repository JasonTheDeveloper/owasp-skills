---
name: infrastructure-vulnerabilities
description: OWASP Top 10 Infrastructure Security Risks (2024) vulnerability knowledge base for identifying, assessing, and remediating security risks in internal IT infrastructure environments.
license: CC-BY-SA-4.0
metadata:
  authors: "OWASP Infrastructure Security Project"
  spec_version: "1.0"
  framework_revision: "1.0.0"
  last_updated: "2026-02-13"
  skill_based_on: "https://github.com/chris-buckley/agnostic-prompt-standard"
  content_based_on: "https://owasp.org/www-project-top-10-infrastructure-security-risks/"
---

# Infrastructure Vulnerabilities — Skill Entry

This `SKILL.md` is the **entrypoint** for the Infrastructure Vulnerabilities skill.

The skill encodes the **OWASP Top 10 Infrastructure Security Risks (2024)** as structured,
machine-readable references that an agent can query to identify, assess, and remediate
infrastructure security risks.

## Normative references (Infrastructure Top 10)

1. [00 Vulnerability Index](references/00-vulnerability-index.md)
2. [01 Outdated Software](references/01-outdated-software.md)
3. [02 Insufficient Threat Detection](references/02-insufficient-threat-detection.md)
4. [03 Insecure Configurations](references/03-insecure-configurations.md)
5. [04 Insecure Resource and User Management](references/04-insecure-resource-user-management.md)
6. [05 Insecure Use of Cryptography](references/05-insecure-use-of-cryptography.md)
7. [06 Insecure Network Access Management](references/06-insecure-network-access-management.md)
8. [07 Insecure Authentication Methods and Default Credentials](references/07-insecure-authentication-default-credentials.md)
9. [08 Information Leakage](references/08-information-leakage.md)
10. [09 Insecure Access to Resources and Management Components](references/09-insecure-access-resources-management-components.md)
11. [10 Insufficient Asset Management and Documentation](references/10-insufficient-asset-management-documentation.md)

## Skill layout

- `SKILL.md` — this file (skill entrypoint).
- `references/` — the Infrastructure Top 10 normative documents.
  - `00-vulnerability-index.md` — master index of all vulnerability identifiers, categories, and cross-references.
  - `01` through `10` — one document per vulnerability aligned with OWASP Infrastructure Security numbering.
- `assets/` — reusable format and constants blocks.
  - `constants/` — vulnerability catalog and category definitions.
    - `constants-infrastructure-catalog-v1.0.0.md`
  - `formats/` — output contract examples.
    - `format-vulnerability-assessment-v1.0.0.md`
    - `format-remediation-checklist-v1.0.0.md`

## Third-Party Attribution

Copyright © OWASP Foundation.
OWASP® Top 10 Infrastructure Security Risks (2024) content is derived from works by the
OWASP Foundation, licensed under CC BY-SA 4.0
(<https://creativecommons.org/licenses/by-sa/4.0/>).
Source: <https://owasp.org/www-project-top-10-infrastructure-security-risks/>
Modifications: Vulnerability descriptions restructured into agent-consumable reference
documents with added detection and remediation guidance.
OWASP® is a registered trademark of the OWASP Foundation. Use does not imply endorsement.
