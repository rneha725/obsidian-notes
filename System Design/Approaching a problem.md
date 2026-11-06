### Interview
- Start with FR -> the user should
	- Choose top 3 FRs to focus, else all is out-of-scope
- then NFR -> the system should
- APIs/core entities
- Then HLD which will cover the FR and APIs 
	- Define the service boundaries here, according to the scale and reads and wrote ratio
	- Service failure patterns.
	- See if they are writing to the same table or different, same table means they might belong to same service. 
- In deeps dives, go to your NFRs and see what bottlenecks you see.

#### Output of the practice questions
- Gaps
- Read about them
- Thinking about alternatives and eliminating them with the tradeoffs

### In Practice

#### NFR
- SCALDS
	- Scalability, Consistency-Availability, Latency, Durability, Security
	- Skip last 2 most of the times, and use fault tolerance instead
- When things go wrong


User related: FR, NFR, Entity, API
Dev Related: Design and tradeoffs

- First draft design should have clear boundaries of components, do not try to do magic here and provide the best solution possible. This is the design which should evolve as you consider the NFRs.
- While evolving your design proactively consider all the possible scenarios and discuss the tradeoffs.
