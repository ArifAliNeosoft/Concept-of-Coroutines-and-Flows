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
