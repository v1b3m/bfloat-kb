We've run into an issue with claude code where a session errors leaving the chat instance broken, new messages will just throw the same error. In an instance like this, the user should be able to just start a new session in the same chat.

1. What are the best ways for the user to start a new session? This needs to be intuitive

### The error in question

```
API Error: 400 
{
  "type": "error",
  "error": {
    "type": "invalid_request_error",
    "message": "messages.206.content.O.tool_re sult.content.0.image.source.base64: image cannot be empty"
  },
  "request_id": "req_011CYAKxv38Bwe8iWHnKft5m"
}

```

All subsequent messages from the user will get this as a response. I am not so sure what causes this.

---

My colleague said they were able to fix this for a specific session by having claude code dig into the session on their machine and fix whatever was broken. They are still not able to provide much more detail.

---

### What can we do?

- We should allow users to have multiple chat sessions for the same project like in cursor.
    
- Each session will be restricted to one agent (either claude or codex) such that if a user wants to start using codex in their project when they feel claude is not solving the problem, they should just start a new session
- 