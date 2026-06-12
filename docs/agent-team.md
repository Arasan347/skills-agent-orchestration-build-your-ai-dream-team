# Agent team

This project uses a custom agent team to build Mona's Project Pulse dashboard:

- Orchestrator — Claude Opus 4.7 (copilot). Coordinates Planner, Coder, and Designer; breaks work into phases and delegates tasks. Definition: .github/agents/orchestrator.agent.md
- Planner — Claude Opus 4.7 (copilot). Researches the repository and produces implementation plans, file assignments, dependencies, and validation guidance. Definition: .github/agents/planner.agent.md
- Coder — GPT-5.5 (copilot). Implements code, creates runnable app support (e.g., .vscode/launch.json), and validates behavior. Definition: .github/agents/coder.agent.md
- Designer — Gemini 3.1 Pro (copilot). Provides UI/UX, accessibility, interaction flow, and visual design for the dashboard. Definition: .github/agents/designer.agent.md

Work is coordinated using the GitHub Copilot CLI running in a Codespace; the CLI orchestrates agents while the learner controls git operations (staging, commits, and pushes).