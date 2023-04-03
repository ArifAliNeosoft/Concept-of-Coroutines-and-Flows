# Concept-of-Flows

## Topics

1. Kotlin Flow API :- Flow Builder, Operator, Collector
2. flowOn, dispatchers
3. Operators
- filter
- map
- zip
- flatMapConcat
- retry
- debounce
- distinctUntilChanged
- flatMapLatest

4. Terminal Operators
5. Cold Flow vs Hot Flow
6. "StateFlow, SharedFlow, Callback Flow,ChannelFlow"

## 1.Kotlin Flow API
- Flows are built on top of coroutines and can provide multiple values.
- In coroutines, a flow is a type that can emit multiple values sequentially, as opposed to suspend functions that return only a single value. 
-  A flow is conceptually a stream of data that can be computed asynchronously. The emitted values must be of the same type. For example, a Flow<Int> is a flow that emits integer values.
  
  #### Flow Builder -To create flows, use the flow builder APIs.
   ### flow {}
  
             - flow builder function creates a new flow.
       - using the emit function to emit new values into the stream of data.
       - Values are collected from the flow using a collect function(Collector).
       - Flows are cold streams similar to sequences — the code inside a flow builder does not run until the flow is collected. 
      
  
> fun simple(): Flow<Int> = flow { 
> println("Flow started")
> for (i in 1..3) {
> delay(100)
> emit(i)
> }
> }
>
> fun main() = runBlocking<Unit> {
> println("Calling simple function...")
> val flow = simple()
> println("Calling collect...")
> flow.collect { value -> println(value) } 
> println("Calling collect again...")
> flow.collect { value -> println(value) } 
> }

  ### flowOf {}
  
  
        - flowOf builder defines a flow that emits a fixed set of values.
  
> (1..3).asFlow().collect { value -> println(value) }

