### A

I should be able to send a response back to the coding agent with a screenshot of the app preview.

We could have a nice UI where it is easy to attach these screenshots to the chat. Perhaps right where the preview is, we can attach a button of some sorts.

Make research on how other similar AI app generators handle this.

### B

The other option is for the agent to view the app itself and verify its changes. This should not be hard to implement.

More often than not I have to manually snap the app then attach that image, we shouldn't have to do this.

sending attachments automatically all the time may increase token usage, so maybe if the user says the UI is broken or something, we can automatically read it via like an mcp tool(check the implementation we have for terminal sessions)

## Notes

- First explore options in plan mode before any code changes.
- 