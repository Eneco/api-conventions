## Authentication

   **Principle: use OAuth 2.0**  
   **Reason:** Web and mobile apps do not have to share passwords. After an initial login, service calls are executed using an accesstoken. Accesstokens have to be renewed regularly and can be revoked by the server in case of security issues.

OAuth is not that hard to implement when using client SDK’s.
