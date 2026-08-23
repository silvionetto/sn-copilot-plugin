# sn-copilot-plugin

Copilot CLI plugin for an **AI Squad** with specialized agents and optional shared skills.

## Squad

- **Project Manager** — orchestrates the squad and routes work
- **System Analyst** — defines requirements and acceptance criteria
- **Software Architect** — defines solution structure and technical decisions
- **Java Developer** — implements Spring Boot/backend changes
- **Tester** — defines and validates quality coverage
- **Platform Engineer** — handles delivery, runtime, and operations concerns
- **Support** — triages issues and production feedback

## Structure

```text
sn-copilot-plugin/
├── plugin.json
├── agents/
│   ├── project-manager.agent.md
│   ├── system-analyst.agent.md
│   ├── software-architect.agent.md
│   ├── java-developer.agent.md
│   ├── tester.agent.md
│   ├── platform-engineer.agent.md
│   └── support.agent.md
└── skills/
```

## Implementation guidance

1. Add one `.agent.md` file per role under `agents/`.
2. Use the Project Manager as the default entry point.
3. Keep role boundaries narrow so analysis, architecture, implementation, testing, platform, and support stay distinct.
4. Add shared skills only when multiple agents need the same prompt logic.

## Collaboration model

The squad is designed around a **Project Manager-orchestrated** workflow:

1. Project Manager receives the request and plans the work
2. System Analyst refines scope, requirements, and acceptance criteria
3. Software Architect defines the technical approach for non-trivial work
4. Java Developer and Platform Engineer contribute implementation and delivery guidance
5. Tester validates coverage and quality expectations
6. Support is engaged for issue triage and production-oriented requests

Each specialist should hand off a compact result with objective, assumptions, findings, risks, and the recommended next recipient.

## Agent summary

| Agent | Primary responsibility | Typical handoff to |
| --- | --- | --- |
| Project Manager | Coordination, scope management, sequencing | Any specialist |
| System Analyst | Requirements, acceptance criteria, constraints | Software Architect, Tester |
| Software Architect | Architecture, technical decisions, slicing | Java Developer, Platform Engineer |
| Java Developer | Spring Boot/backend implementation guidance | Tester, Project Manager |
| Tester | Test strategy, validation, quality risks | Project Manager |
| Platform Engineer | Delivery, runtime, observability, ops | Tester, Project Manager |
| Support | Triage, reproduction, operational feedback | System Analyst, Project Manager |

## Current manifest

- **Name:** `sn-copilot-plugin`
- **Description:** Copilot CLI plugin for an AI Squad with specialized agents and shared skills
- **Version:** `0.1.0`
- **License:** `UNLICENSED`
- **Copilot engine:** `>=1.0.0`

## Plugin setup

Install locally while testing:

```shell
copilot plugin install ./sn-copilot-plugin
```

Then verify it loaded:

```shell
copilot plugin list
```

## Notes

- Reinstall after local changes because plugins are cached.
- Shared skills are still optional and intentionally not added yet because the current prompts are small and role-specific.
