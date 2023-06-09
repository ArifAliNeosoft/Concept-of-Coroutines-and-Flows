## 3.Terminal Operator
- Terminal operators are the operators that actually start the flow by connecting the flow builder, operators with the collector.

#### collect :
  -  It is the most basic terminal operator. It collects values from a Flow and executes an action with each item.
  -  If every update is important like state, view, preferences updates, etc , collect should be used .
  
>  lifecycleScope.launch {
>            (1..5).asFlow()
>                .filter {
>                   it % 2 == 0
>                }
>               .map {
>                    it * it
>               }.collect {
>                    Log.d("Terminal Operators", " Collect : $it")
>              }
>     }

#### collectLatest :
  -  CollectLatest will stop current work to collect latest value.
  -  Terminal flow operator that collects the given flow with a provided action. The crucial difference from collect is that when the original flow emits a new value then the action block for the previous value is cancelled.
  -  If some updates can be overridden with no loss , like database updates , collectLatest should be used.
  
>  flow {
>    emit(1)
>    delay(50)
>    emit(2)
> }.collectLatest { value ->
>    println("Collecting $value")
>    delay(100) // Emulate work
>    println("$value collected")
> }
> Output :  "Collecting 1, Collecting 2, 2 collected"
> 

#### first() 
- Returns only the first emission of the flow.

> lifecycleScope.launch {
>            val item = (13..20).asFlow().first { it % 2 == 0 }
>            Log.d("Terminal Operators", " first : $item")
>        }

#### last() 
- Returns only the last emission of the flow.

> lifecycleScope.launch {
>            val item = (13..20).asFlow().last()
>                        Log.d("Terminal Operators", " last : $item")
>        }

#### toList(), toSet() 
- operators wait until the flow completes and then all the emissions will be returned as a list or set.

> val flowNumbers = flow {
>            delay(100)
>            emit(1)
>            delay(100)
>            emit(2)
>        }
>
>        lifecycleScope.launch {
>            val item = flowNumbers.toSet()
>            Log.d("Terminal Operators", " toSet : $item")
>        }
>
>        lifecycleScope.launch {
>            val item = flowNumbers.toList()
>            Log.d("Terminal Operators", " toList : $item")
>        }


#### reduce { … } 
- Accumulates value starting with the first element and applying operation to current accumulator value and each element.

>lifecycleScope.launch {
>            val sum = (1..5).asFlow()
>                .reduce { a, b -> a + b } 
>            Log.d("Terminal Operators", " reduce : $sum")
>        }
>
> Output: reduce =15


#### fold( ){ … }
- to perform operations like summing up all of the emitted values of a flow.

>  lifecycleScope.launch {
>            val sum = (1..10).asFlow()
>               .fold(10){ result, value -> result + value }//.reduce { a, b -> a + b }
>           Log.d("Terminal Operators", " fold : $sum")
>        }
>        
