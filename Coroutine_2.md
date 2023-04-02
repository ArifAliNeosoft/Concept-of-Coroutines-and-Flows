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
  
  
  CoroutineScope 
  - An interface that consists of a sole property - val coroutineContext:CoroutineContext.
  > Note: We must start all co-routines within scope.
  - To keep track of coroutines lifecycle ,can cancel them and handle error or exceptions thrown by the coroutines.
  - To predict the lifecycle of the coroutines.
  -Acts as reference to the coroutine context.
  
#### GlobalScope 
—  To launch top-level coroutines which are operating on the whole application lifetime, we can use GlobalScope.launch { }. However, this is not ideal.
#### MainScope
— This will launch the coroutine in the main thread (UI thread) by default when using MainScope().launch { }.
#### CoroutineScope
— This allows you to define a custom scope by providing your own context e.g. CoroutineScope(Dispatchers.IO).launch { }.
#### LifecycleScope
— This is an Android-specific coroutine scope that ties the scope Android lifecycle (i.e. ability to automatically terminate upon end of Android Activity lifecycle.
#### ViewModelScope
=  This scope will live as long the view model is alive.
  
 
