There's this IDE error:

```
[Error] Error while parsing the 'sandbox' attribute: 'allow-presentation' is an invalid sandbox flag.
	setValueForAttribute (react-dom_client.js:1452)
	setProp (react-dom_client.js:14495:157)
	setInitialProperties (react-dom_client.js:14784:106)
	completeWork (react-dom_client.js:9077)
	runWithFiberInDEV (react-dom_client.js:999)
	completeUnitOfWork (react-dom_client.js:12669)
	performUnitOfWork (react-dom_client.js:12575)
	workLoopSync (react-dom_client.js:12424)
	renderRootSync (react-dom_client.js:12408)
	performWorkOnRoot (react-dom_client.js:11766:204)
	performWorkOnRootViaSchedulerTask (react-dom_client.js:13505)
	performWorkUntilDeadline (react-dom_client.js:36)
```




## Progress (2026-02-26)
- [x] Removed invalid allow-presentation sandbox flag from preview iframes.

## Outcome
Sandbox parsing error is resolved.

## Commit
- 9c5b0ec