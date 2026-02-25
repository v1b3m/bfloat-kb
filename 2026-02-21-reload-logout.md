We recently worked on a bug where sessions were getting persisted even when a different user logged into the app.

This may have somehow introduced this bug where simply reloading (cmd + r) logs out the current user.

---

It looks like your fix works, however, make sure it doesn't reintroduce the original bug

---

Finally, check for any edge cases we might have missed.

---

