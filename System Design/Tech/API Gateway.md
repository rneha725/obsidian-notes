## Overview

API gateway helps in request routing the the backend services. It is used and popularised by microservice architecture. 

An API gateway sits between a client/(internal service) and backend services.
So the client is now not responsible for connecting the correct service.

Load balancers are one step before the API gateways, it is okay to state only the API gateways in interviews.

### Need of API gateways

1. Validating the client request
2. Applying the middleware like auth, rate limiting, logging monitoring, ip white/blacklisting
3. Communicating with the backend service in the protocol acceptable
4. Getting response from the backend service and caching it, if needed
5. Transforms the response in client language

### Scaling
We need scaling in 2 scenarios: managing load, distribution

#### Managing load

Increase instances as API gateways are stateless.

#### Distribution

Near the users.
