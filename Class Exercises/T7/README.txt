1. Architecture Overview
The system simulates a distributed Serverless environment based on Function as a Service (FaaS) principles. The architecture is designed to demonstrate the following key characteristics:  
+1

Stateless Functions: Each function invocation (handle) is completely independent. Functions do not rely on local memory state between executions.  

Event-Driven Architecture: Functions do not run continuously in the background; they are triggered solely by specific events (e.g., a document indexing event or a search query event).  

Single Responsibility: There is a strict separation of concerns. The indexing function is responsible only for text processing and database building, while the search function handles only retrieval, filtering, and ranking.  
+1

2. Service Documentation
The system consists of three main components (classes):  

A. IndexerFunction

Role: Processes and indexes new documents in the system.  
+1

Input (Event): A dictionary containing a document_id and textual content.  

Ranking Mechanism: To enable advanced relevance ranking, the function builds an Inverted Index that includes Term Frequency. Instead of merely storing a list of documents where a word appears, it records how many times the word appears in each specific document.

B. SearcherFunction

Role: The core search and retrieval engine.  

Input (Event): A dictionary containing a query and the desired logical operator (default is AND).  

Logic & Operators:

AND: Performs an intersection, returning only documents that contain all the search terms.

OR: Performs a union, returning documents that contain at least one of the search terms.

Ranking Mechanism: For each document that passes the logical filter, the function calculates a score by summing the term frequencies from the index. Finally, results are sorted in descending order (highest score first).

C. FaaSSimulator

Role: Simulates the cloud management platform (similar to an API Gateway and container orchestrator).  

Main Method: invoke(function_name, event) - Receives a function name and an event, increments the global invocations counter, and routes the event to the appropriate function in isolation.  

3. Usage Description
To use the system, you first initialize the FaaSSimulator. Then, you can send indexing events by invoking the indexer function with a document ID and content. Once documents are indexed, you can perform searches by invoking the searcher function with a query string and the desired logical operator (AND/OR). The simulator will route these requests and return the results.
