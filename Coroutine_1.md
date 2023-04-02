# Concept-of-Coroutines

## Topics

1. Asynchronous Programming
2. Coroutines

- How do Coroutines different from threads?
- Coroutine Features

3. Coroutine Builders

- launch{}
- async{}
- launch and async differences
- runBlocking

4. withContext()
5. Dispatchers
6. Scopes

- lifecycleScope
- viewModelScope
- CoroutineScope
- coroutineScope Vs CoroutineScope
- GlobalScope
- supervisorScope

7. CoroutineContext
8. job
9. Cancel coroutines

- suspendCoroutine
- suspendCancellableCoroutine

## 1. Asynchronous Programming

1) User can move from one task to another task before the previous one finishes.
2) It increases the amount of work that your app can perform in parallel.

Note: Android is a single thread platform, By default, everything runs on the main thread or UI thread. Android application needs to perform some non UI operations like (Network call, I/O operations).
If we run every task on UI thread and app will take time to complete and if app freezes more than 5 seconds then app control will get ANR (Application not responding ) Dialog which means the app is blocked and ultimately app is crashed.

Many approaches to solve this problem:
Threading , Callbacks , RxJava , Futures and Promises.

But Kotlin's recommended  ,most efficient approach to handle asynchronous code is Coroutines-
which is the idea of suspendable computations, i.e. the idea that a function can suspend its execution at some point and resume later on.


## 2.What are Coroutines?

### Coroutines =Co + Routines 
Here, Co means cooperation and Routines means functions.It means that when functions cooperate with each other.

Coroutines build upon regular functions by adding two new operations. In addition to invoke (or call) and return, coroutines add suspend and resume.

- suspend — pause the execution of the current coroutine, saving all local variables
- resume — continue a suspended coroutine from the place it was paused.

> <dl>
> <dt>The official documentation says </dt>
>  
><dd>coroutines are nothing but <em>lightweight threads.</em></dd>
> </dl>
> The thing to remember is :
> 
> coroutines do not replace threads, it’s more like a framework to manage concurrency in a more performant and simple way with its lightweight thread which is written > on top of the actual threading framework to get the most out of it by taking the advantage of cooperative nature of functions.
>
>By lightweight, it means that creating coroutines doesn’t allocate new threads. Instead, they use predefined thread pools and smart scheduling for the purpose of     > which task to execute next and which tasks later.
>
> ### Coroutines were added to Kotlin in version 1.3


### How do Coroutines different from threads?

-	They are executed inside of a thread, so you can start many coroutines inside a single thread.
-	coroutines are suspendable which means we can execute some instructions, pause the coroutine in the middle of execution and let it continue when we wanted to.
-	they can easily change the context which means that a coroutine that was started in one thread can easily switch to another thread.

### Coroutine Features

#### Lightweight: 
One can run many coroutines on a single thread due to support for suspension,which doesn’t block the thread where the coroutine is running. Suspending frees memory    over blocking while supporting multiple concurrent operations.
#### Built-in cancellation support: 
Cancellation is generated automatically through the running coroutine hierarchy.
#### Fewer memory leaks: 
It uses structured concurrency to run operations within a scope.
#### Jetpack integration: 
Many Jetpack libraries include extensions that provide full coroutines support. Some libraries also provide their own coroutine scope that one can use for structured concurrency.

                 
