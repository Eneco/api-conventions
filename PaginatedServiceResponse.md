## Paginated Service Response

   **Principle: allow large collections to be retrieved in chuncks**  
   **Reason:** pagination allows downloading a datastream in separate sections. This avoids large downloads when the user requires to see only a certain part of the list. Additionally, the first part of a list can be shown on screen while other pages are still downloading in the background. This pattern covers many mechanisms like pre-fetching, lazy loading and data-on-demand.

   **NB:** make sure the service is stateless, even in case of paged queries. When paging through the results of a search request, it is tempting to keep the query open for subsequent calls (which may never happen). The load on the servers will become limited by the amount of available database connections. If they stay open for more than one service call, you run out of connections quickly. In the end, caching the results of the query will be the most efficient optimisation in case of pagination.

**Example:**

VERB | URL | Descriptoin |
---- | --- | ----------- |
GET | /dogs?offset=50&count=25 | Get the next 25 items starting from item=50 |
GET | /dogs/count | Retrieve size of collection for scrollbar settings |
