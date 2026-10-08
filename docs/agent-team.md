# Agent team

For Mona's Project Pulse dashboard, the team uses a small set of custom GitHub Copilot agents defined in `.github/agents/` and orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 4.7 (copilot) — Definition: `.github/agents/planner.agent.md` — researches the codebase, reads relevant docs and dependencies, and produces a concrete implementation plan with file ownership, sequencing, validation steps, and risks.
- Designer — Model: Gemini 3.1 Pro (copilot) — Definition: `.github/agents/designer.agent.md` — focuses on dashboard UX, accessibility, information hierarchy, responsive behavior, and the visual polish of the Project Pulse interface.
- Coder — Model: GPT-5.5 (copilot) — Definition: `.github/agents/coder.agent.md` — implements the actual code, fixes bugs, and creates any required runnable app support such as launch configuration within the assigned file scope.
- Orchestrator — Model: Claude Opus 4.7 (copilot) — Definition: `.github/agents/orchestrator.agent.md` — coordinates the Planner, Designer, and Coder, splits work into phases, delegates tasks with explicit file scopes, and verifies the combined result before reporting back.

This setup keeps the work distributed by specialty while using GitHub Copilot CLI in a Codespace as the control layer for planning, delegation, and integration of the dashboard build.
