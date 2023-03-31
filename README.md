# Concept-of-Coroutines-Flows

|             Launch                          |        Async                             | 
| ------------------------------------------- | :--------------------------------------: | 
|The launch nature is fire and forget.        |Async performs a task and return a result.| 
|The launch {} function returns a Job object. |An async {} returns a Deferred<T> object. |  
|Parallel execution of network calls is not   |Parallel execution of network calls is    |
| possible.                                   | possible.                                | 
|launch{} will not block your main thread.    | Async will block the main thread at the  |
|                                             | entry point of the await() function.     |                                      
