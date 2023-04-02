## 3.Coroutine Builders
- Coroutine Builder, a bridge from the normal to the suspending world.
- These builders are extension function of CoroutineScope(without scope cannot create a coroutine)

#### launch{} :
- This builder creates or start a coroutine and returns instance of Job which refers to launched coroutine.
- We can cancel launched coroutine using the cancel method available on Job.

>  public fun CoroutineScope.launch(
> context: CoroutineContext = EmptyCoroutineContext,
>  start: CoroutineStart = CoroutineStart.DEFAULT,
>  block: suspend CoroutineScope.() -> Unit
>  ): Job

#### async{} :
- This builder creates or start a coroutine and returns instance of Deffered which refers to launched coroutine.
- We can cancel launched coroutine using the cancel method available on Job.

>  public fun <T> CoroutineScope.async(
>  context: CoroutineContext = EmptyCoroutineContext,
>  start: CoroutineStart = CoroutineStart.DEFAULT,
>  block: suspend CoroutineScope.() -> T
>  ): Deferred<T>


#### Difference between lauch{} and async{}

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
|6)launch{} can never work like async{}.        |If async{} will not wait for result- can  |
|                                               |work as launch{}                          | 
|7)Ex:Fetch users membership rating and save    |Ex:Fetch two products price from network/ | 
|  into database.                               | databse to compare acc. to user's need.  |       
