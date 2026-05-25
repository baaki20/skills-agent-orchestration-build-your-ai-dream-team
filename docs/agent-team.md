# Agent team

These custom agents will collaborate to build Mona's Project Pulse dashboard:

- Orchestrator — Claude Opus 4.7 (copilot): coordinates Planner, Coder, and Designer; assigns file scopes, phases, and verifies integration. (.github/agents/orchestrator.agent.md)
- Planner — Claude Opus 4.7 (copilot): researches the codebase and produces ordered implementation plans and file assignments. (.github/agents/planner.agent.md)
- Coder — GPT-5.5 (copilot): implements code, validates behavior, and adds runnable app support (e.g., .vscode/launch.json for app). (.github/agents/coder.agent.md)
- Designer — Gemini 3.1 Pro (copilot): creates UI/UX, accessibility, and responsive dashboard styling with deterministic CSS hooks. (.github/agents/designer.agent.md)

Work will be coordinated using GitHub Copilot CLI inside a Codespace; learners retain control of git operations.
