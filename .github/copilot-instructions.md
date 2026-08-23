# Repository instructions

## What this repo is

This repository is a Copilot CLI plugin that exposes an AI Squad of repository-level agents under `agents/` and shared prompt skills under `skills/`. The main user-facing entry point is the **Project Manager** agent; the other agents are narrow specialists for analysis, architecture, implementation, testing, platform, and support.

## How the repo is structured

- `plugin.json` is the plugin manifest and the source of truth for the plugin name, version, engine requirement, and extension registration.
- `agents/*.agent.md` defines the squad roles and their output contracts.
- `skills/` is reserved for shared prompt logic when multiple agents need the same behavior.
- `.github/plugin/marketplace.json` publishes the plugin to Copilot CLI marketplace flows.

## Build, test, and validation

- Install locally for validation: `copilot plugin install ./sn-copilot-plugin`
- Confirm discovery: `copilot plugin list`
- Register the marketplace entry: `copilot plugin marketplace add silvionetto/sn-copilot-plugin`
- There is no project test/build/lint pipeline or package manifest in this repository.

## Key conventions

- Keep the squad role boundaries narrow: the Project Manager coordinates, the System Analyst defines requirements, the Software Architect decides structure, the Java Developer handles backend implementation guidance, the Tester owns validation, the Platform Engineer covers delivery/runtime concerns, and Support handles triage.
- Each specialist should hand off a compact result with objective, assumptions, findings, risks, and the next recipient.
- Prefer adding shared skills only when multiple agents need the same prompt logic; otherwise keep guidance in the individual agent file.
- Use `.github/agents/` as the discovery path expected by Copilot CLI.

## Copilot usage notes

- Use `/agent` in this repository to select one of the exposed squad roles.
- Opening Copilot CLI in this repo is what makes the repository-level agents available.
- After local changes, reinstall the plugin because Copilot plugins are cached.
