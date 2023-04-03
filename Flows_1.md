# Concept-of-Flows

## Topics

1. Kotlin Flow API :- Flow Builder, Operator, Collector
2. flowOn and dispatchers
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
  - #### flow {}  
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

  - #### flowOf()
     - flowOf builder defines a flow that emits a fixed set of values.
> flowOf(4, 2, 5, 1, 7)
> .collect {
>    Log.d(TAG, it.toString())
> }
  
  - #### asFlow()
     - It is an extension function that helps to convert type into flows.
  
> (1..3).asFlow().collect { value -> println(value) }
  
  - #### channelFlow{}
     - This builder creates flow with the elements using send provided by the builder itself.
> 
> channelFlow {
>   (0..10).forEach {
>       send(it)
>    }
> }
> .collect {
>    Log.d(TAG, it.toString())
> }

  - #### emptyFlow()
     - It is used when we want to create an empty flow.
> emptyFlow<Int>()
>       .collect { print(it) }
>
  
## 2.flowOn and Dispatchers
  
- By default, code in builders, intermediate operators and the collection itself is performed in the coroutine context that invoked the terminal operator. This property of flows is called context preservation.
- it is fine for fast-running, non-blocking code, but for some long term operations you might want to execute the code in a different Dispatcher, like Default or IO.
- To change the context, you can use the flowOn() operator
 
>  
> withContext(Dispatchers.Main) {
>   val singleValue = intFlow // will be executed on IO if context wasn't specified before
>       .map { ... } // Will be executed in IO
>        .flowOn(Dispatchers.IO)
>        .filter { ... } // Will be executed in Default
>        .flowOn(Dispatchers.Default)
>        .single() // Will be executed in the Main
> }
>  
  
  
