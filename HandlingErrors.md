## Handling Errors

   **Principle: Use a limited set of status codes (max 10)**  
   **Reason:** most of the 70 HTTPS error codes are not well-known or easily understood.

   **Principle: Return understandable error codes and explanation**  
   **Reason:** Functional client errors are decided by the business logic of a service, where the service behaves as designed. The client should be capable to recover, notify the user, and ideally allow the user to fix the problem.

   **Principle: Indicated if the error is recoverable**  
   **Reason:** Server Exceptions are raised when the client or service does not behave as designed, due to bugs or environment problems (bad request, zero-pointer, lost database connection etc). The user should be notified, but he may not be able to recover and continue.

   **Principle: the client must translate the error code into an end-user readable message**  
   **Reason:** servermessages may be too technical or in the wrong language.

**Examples:**

CODE | explanation if required |
---- | ----------------------- |
200 - OK | - |
201 - Created | - |
304 – Not Modified | - |
400 – Bad Request | - |
401 - Unauthorized | {"status" : "401", "message":"Authenticate","code": 20003, "more info": "http://www.twilio.com/docs/errors/20003"} |
404 – Not Found | - |
500 – Internal Server Error | - |
