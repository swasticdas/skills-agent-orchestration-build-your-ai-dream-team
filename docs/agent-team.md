# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the Planner, Coder, and Designer; breaks work into scoped phases, manages dependencies and parallel tasks, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies requirements, dependencies, risks, and edge cases, and produces an actionable implementation plan. | `.github/agents/planner.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Shapes the dashboard's usability, accessibility, information hierarchy, interaction flow, visual design, and responsive behavior, including Project Pulse styling expectations. | `.github/agents/designer.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements the assigned application logic and runnable-app support, following existing patterns and validating the work. | `.github/agents/coder.agent.md` |
