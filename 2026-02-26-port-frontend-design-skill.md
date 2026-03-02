# Port Frontend Design Skill to IDE

## Description
Generated applications from bfloat-ide had poor UI quality compared to the workbench. The workbench's `frontend-design` skill was more comprehensive and its system prompt routed the agent to invoke `/frontend-design` automatically — the IDE had neither.

## Progress
- [x] Sync `resources/skills/frontend-design/SKILL.md` with the workbench version
- [x] Add Primary Scope, Existing System Rule, Execution Workflow, Acceptance Checklist sections
- [x] Add `FRONTEND_DESIGN_SKILL_PROMPT` to system prompt (routes agent to `/frontend-design` for UI work)
- [x] Add `EXPO_WEB_STYLE_SAFETY_PROMPT` to system prompt
- [x] Add `TOOL_TRANSPARENCY_PROMPT` to system prompt
- [x] Sync `PROJECT_EXPLORATION_PROMPT` with workbench's leaner version
- [x] Bump `version.json` to 4.1.1
- [x] Commit changes

## Decisions
- Kept IDE-specific `SUGGESTIONS_PROMPT` (has "focus on feature additions" guideline tailored to IDE context)
- Kept IDE-specific `MOBILE_PREVIEW_PROMPT` (workbench doesn't have a mobile preview pane)
- Used routing prompt (~60 tokens) instead of inlining full skill (~800 tokens) to save per-message token cost

## Outcome
Ported all missing sections from the workbench frontend-design skill and wired three new system prompt constants so the agent auto-invokes `/frontend-design` for UI work.

Commit: 1fdbc86

## Related
- Parent issue: [[2026-02-26_terrible-UIs]]
