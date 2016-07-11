### Friendly Service Interface design
   **Principle: Keep it simple and consistent**  
   **Reason:** Makes the API easier to discover, easier to understand

   **Principle: Define API’s resource based (=object oriented)**  
   **Reason:** keep number of API names low

   **Principle: use plural nouns consistently  (/dogs, not /dog)**  
   **Reason:** keeps it consistent, it’s all about collections anyway

   **Principle: use concrete names, avoid abstraction**  
   **Reason:** separates services per class,  makes your API explain itself

   **Principle: allow certain fields to be retrieved**  
   **Reason:** allow the developer to reduce bandwidth by selecting just the fields he needs.

**Examples:** 

 URL | clarification |
 ---- | ----------- | 
 /dogs                        | Get the collection |
 /dogs/1234                   | Get item by ID |
 /dogs?color=black&age=3      | Get subset from collection |
/dogs:(id, name, color)       | Retrieve a list of dogs with only their id, name and color |
/dogs?fields=id, name, color  | Retrieve a list of dogs with only their id, name and color |
**NOT:** /objects/type=dog    | Avoid abstraction |

###	Methods and Non-resource based requests

   **Principle: User HTTP verbs for maintenance actions (CRUD)**
   **Reason:** avoid an endless list of action services

Examples: 
   POST /dogs/1234	Create a new object
   GET /dogs/1234	Read an object
   PUT /dogs/1234/name=Rex&….	If exists, Update all fields (not just 1)
   DELETE /dogs/1234
   DELETE /dogs	Delete 1 or all dogs

Any actions become method names like /Users/Login
   Alternative: POST /Session	Look at the end result of the action.
   A login creates a new session  POST
   A logout removes a session  DELETE
   A token refresh updates a session  PUT
   **NOT: /getBlackDogs**	Avoid all these variations

Principle:  only use a standalone verb in case of a non-resource related request
Reason: sometimes there is no resource or collection behind an action. In case of a calculation or translation there may be some immutable tables that are no user mutable part of the application database (text tables, country-table, money exchange rates etc).

Examples: 
GET /convertAmount?from=EUR&to=USD
&amount=100	Calculate dollar amount
GET  /search? query=terrier+red+park+johnson	Complex Search for a specific dog owned by mr. Johnson

Note: Apigee allows developer authorisations for PUT, GET, POST, DELETE. Other Verbs like OPTIONS, HEAD, are not support by Apigee.
3.3	Object associations
Principle: Use no more than 2 levels in your URL
Principle: Add complexity via parameters
Reason:  reduce complexity
Examples: 
GET /owners/1234/dogs	Read all dogs for owner 1234
GET /owners/1234/dogs?location=park	Read all dogs for owner 1234 that are in the park

3.4	API Subdomains

Principle: use 1 internet domain or your production api’s: eneco-prod.eneco.com 
Reason: APIGEE exepcts the pattern: 
http://{org-name}-{env}.domain.extension/………
The org-name is configured in Apigee as the highest level of management (the root of the configuration). The “env” is a configurable  value for the execution environment, which is a sublevel to the Organisation.

Note: Almost every Rest API on the web can be called and discovered via the api.domain.extension subdomain name, which is a matter of convention. The same goes for the developer documentation that is almost always on developers.x.x . But Apigee has set a different standard.
Examples: 
https://eneco-prod.eneco.com/… 	Base URL for api calls to Eneco services
https://oxxio-prod.eneco.com/… 	Base URL for api calls to Oxxio services
http://developers.eneco.com/…	Base URL for developer information

3.5	API Partitioning and versioning
Principle: Devide up your API according to client applications
Reason: Web, App and B2B clients may require different policies, versioning and governance. Different clients may require different versions of the service API
Examples: 
 /mijneneco-app/dogs
/toon-app/dogs
/mijneneco-web/dogs	All of these endpoints on Apigee may point to the same or different backend services

Principle: each service URL has a version indicator
Reason: service calls must be send to the right version. A new app release may point to a new API definition, while older versions of the app are still active. 
The correct set of URL’s is provided at build time, or via a configuration service. 
The version number indicates the version of the API definition (signature), NOT the version of the service implementation it points to. So the code may change without impact on the API version.
NB: do not put the Version in a HTTP header. This makes the API very hard to use from a browser.
Examples: 
…./v02/mijneneco-app/ dogs	Base URL points to Version 2 of the service 
…/v01/mijneneco-web/ dogs	Base URL still points to Version 1 of the service and will upgrade in the future 

Principle: never hardcode the service endpoints into an application
Reason: endpoints change per environment, and should never be modified in the original code files. This would require a checkin and a re-test of the application.
The correct set of URL’s must be  provided at build time, via a config file, or via a configuration service. 

3.6	What Content Type?
Principle: pass the content-type as a parameter 
Reason: Using a parameter instead of a header makes the service easier to test via a browser (if there are multiple response formats)
Examples: 
/dogs.json	Request a JSON response, if not the default
/dogs?format=xml	Request a XML response

3.7	JSON attribute naming
Principle: use Java or JavaScript naming conventions for JSON fields (use Camel case, start with lowercase)
Reason: the names in the JSON show up in your code after you parsed the JSON response into a response object. The JSON field names have become your class’s field names, which you are going to use in your code.
Examples: 
JSON field: createdAt	Results in timestamp=myObject.createdAt;
	

