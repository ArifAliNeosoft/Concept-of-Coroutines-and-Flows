# Concept-of-Flows

## Topics

1. Kotlin Flow API :- Flow Builder, Operator, Collector
2. flowOn and dispatchers
3. Operators
- filter
- map
- zip
- flatMapConcat
- flatMapLatest
- retry
- debounce
- distinctUntilChanged

4. Terminal Operators
5. Cold Flow vs Hot Flow
6. StateFlow and SharedFlow
7. CallbackFlow
8. ChannelFlow

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

#### Operators in Kotlin Flow
- There are two types of operators available inside the Flow: “terminal ” and “intermediate”
  
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
  
## 3. Intermidiate Operators
- Those are applied to the upstream flow or flows and return a downstream flow where further operators can be applied.
- These operators are cold, just like flows are. 
- A call to such an operator is not a suspending function itself. It works quickly, returning the definition of a new transformed flow.
  
  - #### map 
      - Returns a flow containing the results of applying the given transform function to each value of the original flow.
  
> lifecycleScope.launch {
>            namesOneFlow
>                .map { name -> name.length }
>                .collect {
>                    Log.d("Flow Operators", "map: $it")
>                }
>        }
>
  
   - #### filter 
      - Returns a flow containing only values of the original flow that match the given predicate/condition.
  
>   lifecycleScope.launch {
>            namesOneFlow
>                .filter {
>                    it == "Alex"
>                }
>                .collect {
>                    Log.d("Flow Operators", "Filter : $it")
>                }
>        }
  
   - #### take
       - Returns a flow that contains first count elements. When count elements are consumed, the original flow is cancelled. Throws IllegalArgumentException if count is not positive.

> lifecycleScope.launch {
>            namesOneFlow
>                .take(2)
>                .collect {
>                    Log.d("Flow Operators", "take : $it")
>                }
>        }  
  
   - #### zip
        - Zips values from the current flow (this) with other flow using provided transform function applied to each pair of values. The resulting flow completes as soon as one of the flows completes and cancel is called on the remaining flow.
  
> val flow = flowOf(1, 2, 3).onEach { delay(10) }
>        val flow2 = flowOf("a", "b", "c", "d").onEach { delay(15) }
>        lifecycleScope.launch {
>
>            flow.zip(flow2) { i, s -> i.toString() + s }.collect {
>                Log.d("Flow Operators", "zip : $it")
>            }
>        }
>  
>  Output :1a,2b,3c
  
   - #### flatMapConcat
        - Transform each input element into a Source whose elements are then flattened into the output stream through concatenation. This means each source is fully consumed before consumption of the next source starts.
        -  discourage its usage in a regular application-specific flows.
  
>  fun flowFrom(elem: String) = flowOf(1, 2, 3)
>    .onEach { delay(1000) }
>   .map { "${it}_${elem} " }
>
>  suspend fun main() {
>    flowOf("A", "B", "C")
>        .flatMapConcat { flowFrom(it) }
>        .collect { println(it) }
> }
> OutPut:  (1 sec)
>  1_A
> (1 sec)
> 2_A
> (1 sec)
> 3_A
> (1 sec)
> 1_B
> (1 sec)
> 2_B ........ till (1 sec) 3_C
  
     - #### flatMapLatest
          - It forgets about the previous flow once a new one appears. With every new value, the previous flow processing is forgotten. So, if there is no delay between "A", "B" and "C", then you will only see "1_C", "2_C", and "3_C".
  
> fun flowFrom(elem: String) = flowOf(1, 2, 3)
>    .onEach { delay(1000) }
>    .map { "${it}_${elem} " }
>
>  suspend fun main() {
>   flowOf("A", "B", "C")
>       .flatMapLatest { flowFrom(it) }
>       .collect { println(it) }
> }
>  Output:
> (1 sec)
> 1_C
> (1 sec)
> 2_C
> (1 sec)
> 3_C  
  
     - #### retry
          - Retries collection of the given flow up to retries times when an exception that matches the given predicate occurs in the upstream flow. This operator is transparent to exceptions that occur in downstream flow and does not retry on exceptions that are thrown to cancel the flow.
  
      - #### debounce
          - The debounce operator is used with a time constant in this case. When the user types “a”, “ab”, or “abc” in a short period of time, the debounce operator handles the case. As a result, there will be a large number of network calls. However, the user is ultimately interested in the “abc” search result. As a result, the results of “a” and “ab” must be discarded. There should ideally be no network calls for “a” and “ab” because the user typed those in a very short period of time.
  
      - #### distinctUntilChanged
           - The distinctUntilChanged operator is used to avoid duplicate network calls. Let say the last on-going search query was “abc” and the user deleted “c” and again typed “c”. So again it’s “abc”. So if the network call is already going on with the search query “abc”, it will not make the duplicate call again with the search query “abc”. So, distinctUntilChanged suppress duplicate consecutive items emitted by the source.
  
> searchView.getQueryTextChangeStateFlow()
>    .debounce(300)
>    .filter { query ->
>        if (query.isEmpty()) {
>            textViewResult.text = ""
>            return@filter false
>        } else {
>            return@filter true
>        }
>    }
>    .distinctUntilChanged()
>    .flatMapLatest { query ->
>        dataFromNetwork(query)
>            .catch {
>                emitAll(flowOf(""))
>            }
>    }
>    .flowOn(Dispatchers.Default)
>    .collect { result ->
>        textViewResult.text = result
>    }
>  
  
  
  
  
