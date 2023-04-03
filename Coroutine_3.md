## 9.Job
- According to the official documentation,A Job is a cancellable thing with a life-cycle that culminates in its completion.
- Job interface can control coroutines with its functions like : start(),cancel(),join(),invokeOncompletion().
  - join() function is a suspending function.
  - Job blocks all the threads until the coroutine in which it is written or have context finished its work. 
  - when the coroutine gets finishes, lines after the join() function will be executed.
  - cancel() method is used to cancel the coroutine, without waiting for it to finish its work.
- It has states also: new,Active,Completing,Cancelling,Cancelled,Completed.
- It has few properties also like isActive,isCancelled,isCompleted etc.

## 10. Cancellation of Coroutines
- To cancel a coroutine it should be cooperative.
  - 1. Periodically invoke a suspending function(that belongs to kotlinx.coroutines package) that checks for cancellation, like delay(),yield(),withContext(),withTimeout() etc.
  - 2. Explicitly check for the cancellation status within the coroutine
       -CoroutineScope.isActive boolean flag.
       
## 11. suspendCoroutine and suspendCancellableCoroutine
- We have many libraries in our Android Project that provides the callback way to use instead of the Coroutines way.But nowadays we used Coroutine concepts.
- To convert any Callback to Coroutines in Kotlin we use below functions:
#### suspendCoroutine

    - Let us assume we have any Library ,which does any task and whose listener has has 2 functions to get the callback :
    - we want to use this library in the Coroutines way.
>  
> Library.doSomething(object : Listener {
>   override fun onSuccess(result: Result) {
>
>   }
>    override fun onError(throwable: Throwable) {
>    
>    }
> })
 
   - by creating a suspend function as below :


> suspend fun doSomething(): Result {
>    return suspendCoroutine { continuation ->
>        Library.doSomething(object : Listener {
>
>            override fun onSuccess(result: Result) {
>               continuation.resume(result)
>            }
>
>            override fun onError(throwable: Throwable) {
>                continuation.resumeWithException(throwable)
>            }
>
>        })
>    }
> }

  - We have followed the following steps to convert the Callback to Coroutines in Kotlin:

  - Create a suspend function to return the Result.
  - Use suspendCoroutine as the return block.
  - Use continuation.resume(result) for the success.
  - Use continuation.resumeWithException(throwable) for the error. 

#### suspendCancellableCoroutine
    - Suppose the library also supports the cancellation of the task.
    - we can use the cancel() method to cancel the task using the id.
    - let's convert this Callback to the Coroutines way by creating a suspend function as below:

> suspend fun doSomething(): Result {
>    return suspendCancellableCoroutine { continuation ->
>        val id = Library.doSomething(object : Listener {
>
>            override fun onSuccess(result: Result) {
>                continuation.resume(result)
>            }
>
>            override fun onError(throwable: Throwable) {
>                continuation.resumeWithException(throwable)
>            }
>
>        })
>
>        continuation.invokeOnCancellation {
>            Library.cancel(id)
>        }
>    }
> }

  - We have used all steps as for suspendCroutine but used continuation.invokeOnCancellation as our library supports the cancellation of the task.

> Now we can use above converted functions as below
> 
>   launch {
>   val result = doSomething()
> }

## From the official doc: suspendCancellableCoroutine suspends the coroutine like suspendCoroutine but provides a CancellableContinuation to the block.
   - We can use either suspendCancellableCoroutine or suspendCoroutine according to our usecase in the project.
