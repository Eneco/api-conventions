# The API Façade Pattern
**Principle: separate the API and the implementation**

**Reason**: Developers connect to the API management layer, and have no direct interaction with the service implementations. 
Eneco has selected Apigee as it’s cloud-based API manager. In Apigee the configuration can direct an endpoint to another service implementation or another server without impact on the client app. The implementation is basically hidden from the service consumer.
The Apigee façade provides other useful benefits like:
-	Security – it provides authentication and acts as a reverse proxy
-	Governance – maintenance staff and architects can overview the services landscape and related policies
-	Throttling – Apigee may be configured with policies that restrict traffic

# Encapsulation 
**Principle: the service does not trust the client; it enforces all relevant business rules and other non-functional requirements (eg security)**

**Reason**: as a service, you cannot rely on the client code to completely follow the business rules. You can also not know if you are dealing with a malicious client that wants to inject dangerous data or code into your API.

# Scalability and Availability
**Principles: Design Stateless services**

**Reason**: stateful services require the server to keep the allocated memory for that session active until the client returns with a new call (based on a session cookie). Memory will fill up quickly, and the server becomes overloaded and unresponsive. This defeats the performance improvement for which the session was kept active. 
Stateless services release their memory and database connections immediately and allow quick garbage collection. This way the server can handle much more concurrent sessions and stay responsive longer. And if a server fails, the next call from the client will be sent to another available server, instead of the user having to log in again.

# Transactions
**Principle: Design business transactions, not a set of resource updates.**

**Reason**: Service based transactions have no common transaction context that support a Rollback from the database. 
The client developer may want to execute a multi-step transaction to achieve some business value. For instance, placing an order requires confirmation on a list of items, a payment, a confirmation email, a CRM event and an inventory correction. Design a service that does this all in one step. It is faster for the developer to achieve his goal, he doesn’t need to know the order of all steps, and the risk of an incomplete transaction is much smaller.
NB: In case of an error, the service must execute compensating steps to undo the partial transaction.

# Configurability
**Principle: Provide a Startup service for each app**

**Reason**: setup information may change often, and should be retrieved on demand. Hardcoding volatile setup information into an app will cause too many appstore updates, which is annoying to end-users.
**Solution**: execute an initial service call to a fixed (unchanging) endpoint to retrieve current OTAP endpoints, configuration parameters, textual updates (conditions, readme etc). 
The same startup service may contain parameters to block the app (killswitch) or instruct the user to update.

# API Granularity
**Principle: allow a user to reach his goals in a minimal amount of steps**

**Reason**: “Chatty API’s” require many datacalls to achieve a goal. The idea comes from data normalization, but is not a good idea for services. The overhead on a servicecall is relatively high compared to the cost of a larger payload. 
Data API’s may be flexible, but are so granular that it requires multiple steps to retrieve a resultset like a complex “join”. You first need to get a list of element keys, retrieve the records, and resolve some more foreign keys, while the API could have returned the full de-normalized resultset. It is more convenient to provide API ‘s that return the result that most developers will need.

**Principle: Design business transactions, not a set of resource updates.**

**Reason**: Single business service calls are less granular (chatty) and have less chance of failing (see 2.4).
