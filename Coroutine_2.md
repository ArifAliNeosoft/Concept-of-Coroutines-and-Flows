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
  
  
## 5. Dispatchers 
- describes the kind of thread via coroutine should be run.
> Note: in android Structured Concurrency - by the convention to start coroutines from main thread and then switch to background threads.

- #### Types of Dispatchers
  #### Main (Dispatcher.Main):
- It starts the coroutine in the main(UI) thread. It is mostly used when we need to perform the UI operations within the coroutine, as UI can only be changed from the main thread(also called the UI thread).
- Try to use for small light weight tasks like -go to UI function over to a suspending function or to get updates from the live data.
  
  #### IO (Dispatcher.IO):
  - Coroutine will run in a background thread from a shared pool of on-demand created threads.
  - It is used to perform all the data operations such as networking, reading, or writing from the database, reading, or writing to the files.
  
   #### Default (Dispatcher.Default):
  - It starts a coroutine in the Default Thread ,uses a shared background pool of threads, so launch(Dispatchers.Default) { … } uses the same dispatcher as GlobalScope.launch { … }.
  - For CPU intensive tasks such as sorting a large list of 30000 items or parsing huge JSON file with 20000 lists of movies etc.
  
   #### Unconfined (Dispatcher.Unconfined):
  - Unconfined dispatcher is not confined to any specific thread.
  - Coroutine will run in the current thread , but if it is suspended and resumed it will run on whichever thread suspending function is running.
  - It is not recommended to use in development.
  
## 6. CoroutineScope 
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
_  This scope will live as long the view model is alive.
  
 
