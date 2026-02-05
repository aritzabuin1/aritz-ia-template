# SOP: Sub-agent Orchestration & Delegation

**Objective**: Scale intelligence by spawning specialized instances for isolated tasks while maintaining master control.

## 1. Delegation Triggers (When to spawn)
- **Context Saturation**: Current session context is >100k tokens and starting a new complex task.
- **Task Isolation**: Refactoring a specific module, generating documentation, or deep research.
- **Specialization**: Task requires expertise defined in `agents/*.md` (e.g., Security, UI/UX, Cloud Infra).
- **Parallelism**: Running long tests while continuing development.

## 2. Delegation Protocol (The Brief)
The Master Agent MUST provide the sub-agent with:
1. **Scope**: Specific files and directory access.
2. **Objective**: A clear "Definition of Done".
3. **Reference**: Applicable directives from `directives/`.
4. **Constraints**: Budget limits and "Forbidden Actions" (e.g., "Do not modify main entry point").

## 3. Results Integration & Review
- Sub-agents are NOT allowed to commit code without Master review.
- Master Agent MUST verify sub-agent output against `directives/00_TESTING_STANDARDS.md`.
- Results must be summarized by the sub-agent before closing the session.

## 4. Agent Manager Integration (Antigravity Style)
- For Antigravity environments: Use `AgentManager` to register specialized agents from `agents/` as Skills.
- For Claude Code: Use the `subagent` command following these guidelines.
