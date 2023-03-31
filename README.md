# Concept-of-Coroutines-Flows

|             Launch                            |        Async                             | 
| --------------------------------------------- | :--------------------------------------: | 
|1)The launch nature is fire and forget.        |Async performs a task and return a result.| 
|2)The launch {} function returns a Job object. |An async {} returns a Deferred<T> object. |  
|3)Parallel execution of network calls is not   |Parallel execution of network calls is    |
| possible.                                     | possible.                                | 
|4)launch{} will not block your main thread.    | Async will block the main thread at the  |
|                                               | entry point of the await() function.     |
|5)Other parts of the code will execute and not | Other parts of the code cannot execute   |
|  wait for the launch result since launch is   |and have to wait for the result of the    | 
| not a suspend call .                          | await() function.                        |            |6)launch{} can never work like async{}.        |id async{} will not wait for result- can  |
|  wait for the launch result since launch is   |work as launch{}                          | 
|7)Fetch users membership rating and save into  | Fetch two products price from network /  |            |  database.                                    | databse to compare acc. to user's need.  |                        
