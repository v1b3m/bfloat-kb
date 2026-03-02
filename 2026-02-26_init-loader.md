# Init Loader — Generation Overlay

## Description
Port the translucent overlay with rotating humorous messages from bfloat-workbench to the IDE HomePage. Provides better UX feedback during project creation (previously just a disabled ChatInput).

## Progress
- [x] Add messages array and state to HomePage.tsx
- [x] Add rotating interval effect (1800ms cycle)
- [x] Add AnimatePresence + motion.div overlay JSX
- [x] Add CSS classes (overlay, card, spinner, title, message)
- [x] Add `position: relative` to `.home-main` for absolute overlay positioning

## Decisions
- Used `position: absolute` on the overlay (scoped to `.home-main`) rather than `position: fixed` to avoid covering the sidebar
- Reused existing `Loader2` and `motion`/`AnimatePresence` already available in HomePage
- CSS `@keyframes spin` for the spinner — may overlap with Tailwind `animate-spin` if present, but keeps it self-contained

## Outcome
Implemented and committed. Overlay appears on project creation with fade+scale animation, shows rotating messages, and disappears on navigation or error.

Commit: 795841c