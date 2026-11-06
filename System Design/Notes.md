- When locks are needed, we think about a row level lock, but also think of Redis like locks in that. 
	- See in this [article](https://www.hellointerview.com/learn/system-design/problem-breakdowns/ticketmaster#1-how-do-we-improve-the-booking-experience-by-reserving-tickets) how Redis lock gives us flexibility, and we do not need db level expiry time, only in Redis we will have TTL. Also, we do not need as reserved state for the ticket anymore. If some ticket id is in Redis => means ticket is in reserved state. In the ticket table, we will only have available or booked.
- When you have an API which will result in a lot of results, make sure to use pages.
	- For example, search results, feed etc.

##### Considering a Queue
- While considering a queue, think about how will the ack work. When it will be acked.
- Why are we considering a queue? generally to handle traffic so we won't drop events. 
	- Because processing can take time and till then the queue can pile up. 
	- Downstream systems can fail.
	- Retries 
- Hot partition problem: A partition is always consumed by one consumer in a consumer group.
	- We can add salt to distribute the data.