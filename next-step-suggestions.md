explored a bunch of options, two stood out

1. Make a second API call to a lighter LLM say haiku with the message context, and have it generate suggestions
	1. The downside of this is the added cost and latency
2. Have the agent itself return suggestions at the end of its response in `<suggestions></suggestions>` tags
	1. model always has context
	2. no extra cost
	3.