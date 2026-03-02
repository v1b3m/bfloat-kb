The white phone frame looks out of place against the dark background in dark mode. Perhaps, we should make it dark?

However, to some extent I understand that the color inversion might serve to make it fully visible against the background. Perhaps we can make it a little dim so the contrast is not so grim?

### Current state

#### Dark Mode

![[Pasted image 20260226190432.png|300]]

#### Light Mode

![[Screenshot 2026-02-26 at 19.12.15.png]]

It stays white in light mode, the shadow makes it stand out. Perhaps we can also use a dark color along with a shadow in dark mode too?

What do you think?
Use plan mode to plan this out first.

---




## Progress Update (Codex)
- [x] Located mobile frame styling in `app/components/preview/IPhoneFrame.tsx`.
- [x] Added theme-aware frame rendering via `themeStore.resolvedTheme`.
- [x] Applied dark-mode override for `silver` variant to a dimmer graphite gradient.
- [x] Tuned dark-mode frame shadow to reduce harsh contrast on dark background.
- [x] Ran lint on touched file: `pnpm exec eslint app/components/preview/IPhoneFrame.tsx` (pass).

## Decisions (Codex)
- Kept scope strictly to `IPhoneFrame` to avoid side effects in desktop preview and unrelated UI.
- Preserved light mode visual exactly; changed only dark mode for default `silver` frame.
- Did not alter `deep-blue` and `cosmic-orange` variants because they already read as dark-compatible finishes.

## Open Verification
- Visual QA needed in-app for both themes to confirm preferred contrast level.

- Commit: `1133f7b` (Adjust iPhone preview frame for dark mode)