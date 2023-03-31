# Concept-of-Coroutines

## Topics

1. Asynchronous Programming
2. coroutines Vs Coroutine
3. coroutine Builders

- launch{}
- async{}
- launch and async differences
- runBlocking

4.withContext()
5.Dispatchers
6.Scopes

-lifecycleScope
-viewModelScope
-CoroutineScope
-GlobalScope
-supervisorScope

7. contex
8. job
9. Cancel coroutines

-suspendCoroutine
-suspendCancellableCoroutine

## 1. Asynchronous Programming

1)User can move from one task to another task before the previous one finishes.
2) It increases the amount of work that your app can perform in parallel.

Note: Android is a single thread platform, By default, everything runs on the main thread or UI thread. Android application needs to perform some non UI operations like (Network call, I/O operations).
If we run every task on UI thread and app will take time to complete and if app freezes more than 5 seconds then app control will get ANR (Application not responding ) Dialog which means the app is blocked and ultimately app is crashed.

Many approaches to solve this problem:
Threading , Callbacks , RxJava , Futures and Promises.

But Kotlin;s recommended  ,most efficient approach to handle asynchronous code is Coroutines-
which is the idea of suspendable computations, i.e. the idea that a function can suspend its execution at some point and resume later on.




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
| not a suspend call .                          | await() function.                        |
|6)launch{} can never work like async{}.        |id async{} will not wait for result- can  |
|  wait for the launch result since launch is   |work as launch{}                          | 
|7)Fetch users membership rating and save into  | Fetch two products price from network /  | 
|  database.                                    | databse to compare acc. to user's need.  |                        
