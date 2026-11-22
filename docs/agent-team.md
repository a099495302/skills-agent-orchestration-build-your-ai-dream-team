# Agent team

The Project Pulse dashboard is built with a specialist team orchestrated from GitHub Copilot CLI in this Codespace.

## Orchestrator

- **Model:** Claude Opus 5.5 (copilot)
- **Responsibility:** Break down the Project Pulse work, assign explicit file scopes, coordinate the specialist agents, sequence dependent work, and verify the integrated dashboard.
- **Definition:** [.github/agents/orchestrator.agent.md](../.github/agents/orchestrator.agent.md)

## Planner

- **Model:** Claude Opus 5.5 (copilot)
- **Responsibility:** Research the repository and Project Pulse requirements, identify implementation steps and edge cases, define dependencies and parallel work, and return a practical technical plan.
- **Definition:** [.github/agents/planner.agent.md](../.github/agents/planner.agent.md)

## Coder

- **Model:** GPT-5.5 (copilot)
- **Responsibility:** Implement Project Pulse application logic and structure, keep behavior deterministic and testable, and create support configuration such as the launch setup for the dashboard when assigned.
- **Definition:** [.github/agents/coder.agent.md](../.github/agents/coder.agent.md)

## Designer

- **Model:** Gemini 3.7 Flash (copilot)
- **Responsibility:** Define the dashboard's UI and UX, including visual hierarchy, project cards and status badges, accessibility, responsive layout, interaction flow, and polished visual styling.
- **Definition:** [.github/agents/designer.agent.md](../.github/agents/designer.agent.md)

## How the team builds Project Pulse

The Orchestrator starts by giving the Planner Project Pulse's desired outcome. The Planner researches the codebase and produces ordered implementation steps, file assignments, dependencies, parallel work, edge cases, and validation expectations.

The Orchestrator turns that plan into phases, then delegates each specialist a specific file scope. The Designer owns the dashboard's visual and accessibility direction, while the Coder implements the application structure and behavior. The Orchestrator runs independent design and implementation work in parallel only when file scopes do not overlap, and sequences dependencies or shared files. It also assigns launch or tooling setup to the Coder when needed.

After each phase, the Orchestrator checks progress and integrates the specialist outputs. It verifies that the dashboard has clear project cards, status and priority treatment, responsive behavior, accessible interactions, and a usable first view. The team reports blockers and remaining risks instead of hiding uncertainty, and it does not stage, commit, or push changes unless Mona explicitly asks it to do so.
