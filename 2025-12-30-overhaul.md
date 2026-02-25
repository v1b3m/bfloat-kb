We need to do an overhaul of ourr app generation as it's painfully slow.

1. We've seen other app generators be extremely fast

2. One of our slow points are linting and app verification
   
   1. Gather insights from https://manus.im and determine how they do this so fast, we need to replicate their approach

We'll be starting from the backend, and when that's fixed and working, we'll move over to the frontend.



## Queue System

We were thinking of implementing a queue system where claude picks tasks.

### Is this necessary?

1. Queue systems are non-blocking, any available agent can pick tasks from this
   
   1. Though I was thinking the redis worker already kinda works like a queue, so we'll need to think about this some more. Do we improve what we have? Do we overhaul the entire system

2. 



First, I am going to start an app generation session with manus, observe and note whatever insights you have, create a new md file in `/pg` where you'll be adding these insights.








