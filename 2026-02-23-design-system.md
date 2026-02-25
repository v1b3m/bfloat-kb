# Claude-First, Cross-Agent Design Consistency System

## Summary
Build one source of truth for design intent, then bind every coding agent to it through persistent instruction files, scoped rules, MCP design context, and visual quality gates.  
Goal: agents generate UI that is consistent with your preferred design principles without re-prompting every task.

## Research-Based Direction (as of February 23, 2026)
- Claude Code supports persistent project/user memory via `CLAUDE.md`, `.claude/CLAUDE.md`, `.claude/rules/*.md`, with precedence by specificity.
- Codex supports layered `AGENTS.md` and `AGENTS.override.md`, merge order from root to cwd, plus `project_doc_max_bytes` and fallback filenames.
- GitHub Copilot supports repository instructions and path-specific instruction files, and coding agent support includes `AGENTS.md` and `CLAUDE.md`.
- Cursor supports versioned project rules in `.cursor/rules` with scoped application.
- Figma MCP server is available (remote endpoint and desktop mode), enabling design context transfer into agent workflows.
- Playwright supports screenshot-based visual comparison (`toHaveScreenshot`) and configurable diff tolerances for CI enforcement.
- Design Tokens Community Group published stable spec modules (`2025.10`), making token format a strong interoperability base.

## Implementation Plan

## 1. Create a Design Principles Corpus (portable, model-agnostic)
- Define 8–12 explicit principles from your reference apps:
  - Hierarchy and density
  - Spacing rhythm
  - Typography scale and pairing
  - Color usage rules
  - Interaction patterns
  - Motion constraints
  - Accessibility constraints
  - “Never do” anti-patterns
- For each principle, store:
  - Rule statement
  - Rationale
  - Positive example
  - Negative example
  - Verifiable checklist item
- Output artifact: `design-system/principles.md`.

## 2. Encode Principles as Machine-Consumable Contracts
- Create token source in DTCG-compatible format for:
  - Color
  - Typography
  - Spacing
  - Radius
  - Shadow
  - Motion duration/easing
- Create component behavior contracts:
  - Button, input, card, modal, table, nav, empty state
  - States and interaction expectations
- Output artifacts:
  - `design-system/tokens/*.json`
  - `design-system/components/*.md`
  - `design-system/anti-patterns.md`

## 3. Build a Claude-First Instruction Layer
- Root `CLAUDE.md`:
  - Canonical design directives
  - Required workflow for UI work
  - Acceptance checklist
- `.claude/rules/`:
  - `visual-language.md`
  - `component-contracts.md`
  - `accessibility.md`
  - `review-checklist.md`
- Require every UI response to include:
  - “Applied principles” list
  - “Deviations and why” section
  - “Validation commands/tests run” section

## 4. Mirror the Same Rules Across Other Agents
- Codex:
  - `AGENTS.md` at repo root
  - Optional subdirectory overrides with `AGENTS.override.md` for product areas
  - Optional skill package for UI generation/review workflow
- Copilot:
  - `.github/copilot-instructions.md`
  - `.github/instructions/**/*.instructions.md` for scoped stacks
- Cursor:
  - `.cursor/rules/*.mdc` with global + path-scoped rules
- Keep one canonical source and generate agent-specific files from it to avoid drift.

## 5. Connect Design Files Directly (MCP)
- Add Figma MCP server into the development agent stack.
- Use selection-based or link-based flows to pull exact component/variable context.
- Add workflow rule: UI implementation tasks must reference a Figma node/frame ID when applicable.

## 6. Add Non-Negotiable Verification Gates
- Visual regression:
  - Playwright `toHaveScreenshot()` for key surfaces and states
  - Stable environment policy for baseline generation
  - Controlled tolerance config
- Semantic checks:
  - Lint/style checks
  - Accessibility checks
  - Token usage checks (block raw hard-coded values where tokens exist)
- PR policy:
  - Reject if screenshots drift without explicit approval note
  - Reject if token contract violations occur

## 7. Create an Agent Evaluation Harness
- Build a fixed benchmark suite of UI tasks:
  - New screen from prompt
  - Refactor existing screen to principles
  - Add component variant
  - Resolve visual bug without style regression
- Score each run:
  - Principle adherence
  - Token compliance
  - Accessibility
  - Visual diff magnitude
  - Rework required by human reviewer
- Run monthly comparison across Claude/Codex/Copilot/Cursor configs.

## 8. Rollout Sequence
1. Week 1: Principles corpus + tokens + component contracts.
2. Week 2: Claude instruction stack + initial verification checks.
3. Week 3: Port rules to Codex/Copilot/Cursor and normalize outputs.
4. Week 4: MCP + benchmark harness + CI enforcement hardening.

## Public Interfaces, Types, and Contracts
- Persistent instruction files:
  - `CLAUDE.md`
  - `.claude/rules/*.md`
  - `AGENTS.md`
  - `AGENTS.override.md`
  - `.github/copilot-instructions.md`
  - `.github/instructions/**/*.instructions.md`
  - `.cursor/rules/*.mdc`
- Design contracts:
  - DTCG-style token JSON
  - Component contract markdown schema
- CI interfaces:
  - Playwright visual snapshot baselines
  - Rule-check CLI outputs as required PR checks

## Test Cases and Scenarios
1. Agent generates a new page from a plain-language prompt and uses only approved tokens.
2. Agent modifies an existing page and preserves component/state contract behavior.
3. Agent receives conflicting style prompt and correctly follows repository design rules.
4. Agent output passes visual snapshot checks on supported CI environment.
5. Agent handles responsive breakpoints without violating spacing/typography rhythm.
6. Agent proposes deviation and includes explicit rationale plus fallback option.

## Assumptions and Defaults
- Scope chosen: Claude-first system, transferable to other agents.
- Primary target: web app UI (desktop + mobile responsive).
- Default enforcement: CI-blocking for token violations and unapproved visual diffs.
- Default single source of truth: `design-system/` artifacts; agent-specific instruction files are generated mirrors.
- If no Figma link/node is available, agent must use component contracts and mark confidence as reduced.

## Sources
- Claude Code overview: https://code.claude.com/docs
- Claude memory and precedence: https://code.claude.com/docs/en/memory
- Claude settings: https://code.claude.com/docs/en/settings
- Codex AGENTS.md guide: https://developers.openai.com/codex/guides/agents-md
- Codex skills: https://developers.openai.com/codex/skills
- Codex MCP: https://developers.openai.com/codex/mcp
- GitHub Copilot custom instructions overview: https://docs.github.com/en/copilot/how-tos/custom-instructions
- GitHub Copilot tutorial (custom instructions): https://docs.github.com/en/copilot/tutorials/customization-library/custom-instructions/your-first-custom-instructions
- GitHub Copilot coding agent best practices: https://docs.github.com/en/copilot/tutorials/coding-agent/get-the-best-results
- Cursor rules for AI: https://docs.cursor.com/context/rules-for-ai
- Playwright visual comparisons: https://playwright.dev/docs/test-snapshots
- Figma MCP guide: https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Dev-Mode-MCP-Server
- Figma MCP announcement: https://www.figma.com/blog/introducing-figmas-dev-mode-mcp-server/
- W3C Design Tokens Community Group: https://www.w3.org/community/design-tokens/
