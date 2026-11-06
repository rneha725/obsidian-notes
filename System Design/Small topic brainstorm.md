##### How connections are stored and used?
- Not sure, but it is like a voice call, there are file descriptors on both the ends but both ends also ack what has been received on the both ends.
- Analogy: call center
	- A user tries and gets connected to person A. So now Dashboard knows(Redis), that connection1 -> person A. A physical line is connected.
	- They both talk. Say another person B from call center wants to talk to connection1, they do it via forwarding their request to person A.
	- There is filing cabinet where we store every message for connections. Call center people get the relevant note and forward to the connection.
	- If person A faints, another person needs to create this physical connection first.

##### Kafka vs any other Q system usage examples?

##### Configuring Cache TTLs??

##### Pagination:
- on server side, say we want to return the feed of a user, why return every post? user does not need that. 
- Also client side the page will be downloaded, if the content is just to much, it will increase the memory.
- We can also time bound the requests, instead of fetching everything.
- As client feeds can be different, one client cant hog.
- Tradeoffs: new page request means a new request to the server but at the same time we are sending user feed sooner. Also, client have to manage state.
- Usage: fixed offset (can cause duplicates on insertion), Cursor based, we use a incremental values, so we can ignore what comes before it (we need indexes for the filter and the sort)
- Alternative: Streaming (we need single long lived connection, and resume)
##### API where to put what?
- Header: metadata of the request
	- Auth token, Idempotency key,  
- Path Param: resource you are accessing
	- Product page, user page etc.
- Query Param: query to use on a resource
	- GET only has query and path params no body
	- category type, user id etc.
	- sort, filter, page etc.
- Body: data you want to send
	- Have a payload, in case you get query needs to send data use POST

- Never put secrets in query params.
- **GET has no body** in practice. If a search is too complex for query params, use `POST /search` with a body.
- Idempotency key goes in a header.
- **Pagination** uses a query param: `?cursor=xyz&limit=50`. Where we can use a timestamp, say created_at, and get next 50 posts after that. We need to add created_at as an index to do the range queries.
- **API versioning** can go in the path (`/v1/users`, the simplest option) or a header (`Accept: application/vnd.api.v2+json`, which keeps URLs cleaner). Either is fine if you can justify it.