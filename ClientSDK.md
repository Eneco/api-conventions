## Provide a client SDK

   **Principle: make life easier for API consumers by providing Client SDK’s**  
   **Reason:** each API that you publish must be consumed by a client app, which means calling it properly, and interpreting the API response.
   
   Although setting op a HTTP(S) connection, calling your API URL, parsing the result response and processing the data can be done in any programming language, it is far easier and quicker if the API comes with a pre-implemented set of methods that do all this. This set of service adapters and utility functions is called a Software Development Kit. It hides the details and helps the application developer to focus on his functional goals.

   For the API provider it is quite an effort to maintain SDK’s for most popular client languages. At least SDK’s for JavaScript, Java, C#, Objective C and Swift should be considered, to get reasonable coverage of the programming landscape.
