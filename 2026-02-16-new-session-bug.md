We have a bug. When I hit NEW in the chat, it shows the New indicator in the chat. I then send a message which shows up alongside the original messages. However, as soon as the agent starts responding, the message is lost alongside all previous chat messages. it shows "Describe what you want to build" first, then the agent response starts streaming in as if there were no previous messages.

Read `FIX-CHAT-SESSION-ERRORS.md` to understand what we're trying to build.

---

The divider never shows up, we should also look into this.

---

So, when I leave and return to a chat with multiple sessions, only the last session's messages show up. Can we maybe change the system prompt for new sessions (after there was already a session) to first of all, create a summary of the current state of messages from the previous session(s). This way the first message always includes a summary, now when we open the chat, the first message will indicate exactly where we started from (the state of the previous messages summarized).

---

