# OWASP Agent Skills

This repository contains a series of skills covering a range of OWASP related top 10 vulnerabilities.

These vulnerabilities are:

- [OWASP Top 10 for Agentic Applications for 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)
- [OWASP Top 10 CI/CD Securtiy Risks](https://owasp.org/www-project-top-10-ci-cd-security-risks/)
- [OWASP Docker Top 10](https://owasp.org/www-project-docker-top-10/)
- [OWASP Top 10 Infrastructure Security Risks](https://owasp.org/www-project-top-10-infrastructure-security-risks/)
- [OWASP Top 10 for LLM Applications 2025](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/)
- [OWASP MCP Top 10](https://owasp.org/www-project-mcp-top-10/)
- [OWASP Machine Learning Security Top Ten](https://owasp.org/www-project-machine-learning-security-top-10/)
- [OWASP Mobile Top 10](https://owasp.org/www-project-mobile-top-10/)
- [OWASP Top 10 Risks for Open Source Software](https://owasp.org/www-project-open-source-software-top-10/)
- [OWASP Serverless Top 10](https://owasp.org/www-project-serverless-top-10/)
- [OWASP Top 10 2025](https://owasp.org/Top10/2025/)

Skills are based off [Agnostic Prompt Standard (APS)](https://github.com/chris-buckley/agnostic-prompt-standard)

To use skills in Visual Studio Code be sure to enable `chat.useAgentSkills`

## Vulnerability Scanner Agent Flow

```mermaid
flowchart TD
    Start([🚀 Start]) --> Discover

    subgraph Step1["Step 1 — Discover the Codebase"]
        Discover[Scan repository structure] --> Signals{Detect technology signals}
        Signals --> Web["Web app code<br/>(HTML/JS/CSS, REST APIs)"]
        Signals --> CICD["CI/CD pipelines<br/>(.github/workflows, Jenkinsfile)"]
        Signals --> Docker["Dockerfiles &<br/>container manifests"]
        Signals --> Infra["Infrastructure-as-Code<br/>(Terraform, Bicep, Ansible)"]
        Signals --> LLM["LLM/AI app code<br/>(prompt templates, AI chains)"]
        Signals --> MCP["MCP server/client code<br/>& tool definitions"]
        Signals --> ML["ML training/inference<br/>code & model files"]
        Signals --> Mobile["Mobile app code<br/>(Android, iOS, Flutter)"]
        Signals --> OSS["Dependency manifests<br/>(package.json, requirements.txt)"]
        Signals --> Serverless["Serverless functions<br/>(Lambda, Azure Functions)"]
        Signals --> Agentic["Agentic AI orchestration<br/>(multi-agent, tool-use loops)"]
        Web & CICD & Docker & Infra & LLM & MCP & ML & Mobile & OSS & Serverless & Agentic --> Profile[Compile codebase profile<br/>& select applicable skills]
    end

    Profile --> Delegate

    subgraph Step2["Step 2 — Delegate to Sub-Agents"]
        Delegate[For each selected skill] --> Launch["Launch sub-agent<br/>via runSubagent"]
        Launch --> ReadSkill["Sub-agent reads<br/>SKILL.md entrypoint"]
        ReadSkill --> ReadIndex["Read vulnerability index<br/>(00-vulnerability-index.md)"]
        ReadIndex --> Assess["For each vulnerability ID:<br/>read reference file &<br/>examine codebase"]
        Assess --> Classify["Assign status<br/>(PASS / FAIL / PARTIAL / NOT_ASSESSED)<br/>& severity"]
        Classify --> Return["Return structured findings:<br/>• Skill Metadata<br/>• Findings Table<br/>• Detailed Remediation"]
    end

    Return --> Collate

    subgraph Step3["Step 3 — Collate Results"]
        Collate[Aggregate all sub-agent findings] --> Group[Group findings by<br/>skill / OWASP framework]
        Group --> Counts["Compute summary counts<br/>• Total PASS/FAIL/PARTIAL/NOT_ASSESSED<br/>• Severity breakdown"]
    end

    Counts --> Generate

    subgraph Step4["Step 4 — Generate Report"]
        Generate[Build Markdown report] --> Sections["Assemble sections:<br/>1. Executive Summary<br/>2. Summary Counts & Severity<br/>3. Findings by Framework<br/>4. Detailed Remediation<br/>5. Remediation Checklist<br/>6. Appendix — Skills Used"]
        Sections --> Save["Save to docs/reports/<br/>vulnerability-report-YYYY-MM-DD.md"]
    end

    Save --> Done([✅ Report saved])

    style Step1 fill:#1a1a2e,stroke:#e94560,color:#fff
    style Step2 fill:#1a1a2e,stroke:#0f3460,color:#fff
    style Step3 fill:#1a1a2e,stroke:#16213e,color:#fff
    style Step4 fill:#1a1a2e,stroke:#533483,color:#fff
    style Start fill:#e94560,stroke:#e94560,color:#fff
    style Done fill:#0f3460,stroke:#0f3460,color:#fff
```

## Legal

This project is licensed under the [MIT License](./LICENSE).

### Licensing

Most content in this repository is covered by the MIT License. Certain skill content
derived from OWASP Foundation publications is licensed under either
[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) or
[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). Each affected
skill identifies its license in frontmatter and includes a Third-Party Attribution
section. See [THIRD-PARTY-NOTICES](./THIRD-PARTY-NOTICES) for full details.
