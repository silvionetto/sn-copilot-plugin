# sn-copilot-plugin

Copilot CLI plugin for an **AI Squad** with specialized agents and optional shared skills.

## Squad

- **Project Manager** — orchestrates the squad and routes work
- **System Analyst** — defines requirements and acceptance criteria
- **Software Architect** — defines solution structure and technical decisions
- **Security Engineer** — reviews dependency security risk and mitigation options
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
│   ├── security-engineer.agent.md
│   ├── java-developer.agent.md
│   ├── tester.agent.md
│   ├── platform-engineer.agent.md
│   └── support.agent.md
├── com.github.copilot/
│   └── agents/
│       ├── project-manager.md
│       ├── system-analyst.md
│       ├── software-architect.md
│       ├── security-engineer.md
│       ├── java-developer.md
│       ├── tester.md
│       ├── platform-engineer.md
│       └── support.md
├── skills/
└── .github/
    └── plugin/
        ├── marketplace.json
        ├── plugin.json
        ├── agents/
        ├── com.github.copilot/
        └── skills/
```

## Implementation guidance

1. Add one `.agent.md` file per role under `agents/`.
2. Use the Project Manager as the default entry point.
3. Mirror each source agent into `com.github.copilot/agents/*.md` and `.github/plugin/com.github.copilot/agents/*.md`.
4. Keep role boundaries narrow so analysis, architecture, implementation, testing, platform, and support stay distinct.
5. Add shared skills only when multiple agents need the same prompt logic.

## Collaboration model

The squad is designed around a **Project Manager-orchestrated** workflow:

1. Project Manager receives the request and plans the work
2. System Analyst refines scope, requirements, and acceptance criteria
3. Software Architect defines the technical approach for non-trivial work
4. Security Engineer assesses dependency and component security risk when relevant
5. Java Developer and Platform Engineer contribute implementation and delivery guidance
6. Tester validates coverage and quality expectations
7. Support is engaged for issue triage and production-oriented requests

Each specialist should hand off a compact result with objective, assumptions, findings, risks, and the recommended next recipient.

## Agent summary

| Agent | Primary responsibility | Typical handoff to |
| --- | --- | --- |
| Project Manager | Coordination, scope management, sequencing | Any specialist |
| System Analyst | Requirements, acceptance criteria, constraints | Software Architect, Tester |
| Software Architect | Architecture, technical decisions, slicing | Java Developer, Platform Engineer |
| Security Engineer | Dependency security review, vulnerability triage, mitigations | Platform Engineer, Java Developer, Project Manager |
| Java Developer | Spring Boot/backend implementation guidance | Tester, Project Manager |
| Tester | Test strategy, validation, quality risks | Project Manager |
| Platform Engineer | Delivery, runtime, observability, ops | Tester, Project Manager |
| Support | Triage, reproduction, operational feedback | System Analyst, Project Manager |

## Current manifest

- **Name:** `sn-copilot-plugin`
- **Description:** Copilot CLI plugin for an AI Squad with specialized agents and shared skills
- **Version:** `0.1.13`
- **License:** `UNLICENSED`
- **Copilot engine:** `>=1.0.0`

## Agents

This repo exposes these agents to Copilot CLI through the documented repository-level agent location:

- `/agent project-manager`
- `/agent system-analyst`
- `/agent software-architect`
- `/agent security-engineer`
- `/agent java-developer`
- `/agent tester`
- `/agent platform-engineer`
- `/agent support`

Use `/agent` in this repository, then select one of the above roles.

When the plugin is installed, Copilot CLI registers them under the plugin namespace, for example:

```shell
copilot --agent sn-copilot-plugin:project-manager
```

Bare agent IDs like `project-manager` do not resolve from the installed plugin on Copilot CLI 1.0.82.

## Plugin setup

Install locally while testing:

```shell
copilot plugin install ./sn-copilot-plugin
```

Then verify it loaded:

```shell
copilot plugin list
```

The repository root is kept as a valid direct-install plugin layout, with `plugin.json` at the top level and runtime agent copies under `com.github.copilot/agents/`.

## Marketplace

This repository includes a marketplace manifest at `.github/plugin/marketplace.json` and a packaged plugin subtree at `.github/plugin/plugin.json`.

You can register it with Copilot CLI using:

```shell
copilot plugin marketplace add silvionetto/sn-copilot-plugin
```

Then install the plugin from the marketplace using:

```shell
copilot plugin install sn-copilot-plugin@sn-copilot-plugin-marketplace
```

The marketplace manifest uses Copilot CLI's required marketplace schema: a top-level marketplace `name`, `owner`, optional `metadata`, and a `plugins` array containing the published plugin entry.

Marketplace installs now point at `.github/plugin`, which gives Copilot a flattened package root containing `plugin.json`, `skills/`, and runtime agent copies under `com.github.copilot/agents/`.

On Copilot CLI 1.0.82, the runtime logs warn that plugins declaring the Agent Plugins v1 `$schema` read agents only from `com.github.copilot/agents`, not from the plugin-root `agents/` directory. This repository therefore keeps `agents/*.agent.md` as the authoring source and materializes the runtime copies in both install layouts.

Installed plugin agents resolve as `sn-copilot-plugin:<agent-name>`, such as `sn-copilot-plugin:project-manager`.

## Releases

Pushing to `main` now runs `.github/workflows/release.yml`, which reads `plugin.json` and creates a GitHub Release tagged as `v<version>`.

To publish a new release, bump the `version` in `plugin.json` before merging or pushing to `main`. If the matching tag already exists, the workflow exits without creating a duplicate release.

## Validation workflow

Pull requests and pushes to `main` now run `.github/workflows/validate-plugin.yml` to catch manifest and documentation drift before release.

The workflow verifies:

- `plugin.json`, `.github/plugin/plugin.json`, and `.github/plugin/marketplace.json` stay in sync
- every `agents/*.agent.md` file is mirrored into `.github/plugin/agents/*.agent.md`
- every `agents/*.agent.md` file is mirrored into `com.github.copilot/agents/*.md`
- every `agents/*.agent.md` file is mirrored into `.github/plugin/com.github.copilot/agents/*.md`
- every `agents/*.agent.md` file is documented in the `/agent` list
- the README manifest version matches `plugin.json`
- the README documents the packaged `.github/plugin` layout and `com.github.copilot/agents` runtime path

## Notes

- Reinstall after local changes because plugins are cached.
- If an older marketplace install is already cached, reinstall or refresh the cached plugin so Copilot picks up the current `com.github.copilot/agents/` runtime files.
- Keep `agents/` as the source of truth and mirror runtime copies under `com.github.copilot/agents/` and `.github/plugin/com.github.copilot/agents/`.
- Shared skills are still optional and intentionally not added yet because the current prompts are small and role-specific.
