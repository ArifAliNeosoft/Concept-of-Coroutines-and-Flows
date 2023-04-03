## 5.Cold Flow vs Hot Flow

|             Cold Flow                         |            Hot Flow                          | 
| --------------------------------------------- | :------------------------------------------: | 
|1)It emits data only when there is a collector.|It emits data even when there is no collector.| 
|2)It does not store data.                      |It can store data.                            |  
|3)It can't have multiple collectors.           |It can have multiple collectors.              |
|                                               |                                              | 

- In Cold Flow, in the case of multiple collectors, the complete flow will begin from the beginning for each one of the collectors,
  do the task and emit the values to their corresponding collectors. It's like 1-to-1 mapping. 1 Flow for 1 Collector.
  It means a cold flow can't have multiple collectors as it will create a new flow for each of the collectors.
  
- In Hot Flow, in the case of multiple collectors, the flow will keep on emitting the values, collectors get the values from where they have started collecting.
 It's like 1-to-N mapping. 1 Flow for N Collectors. It means a hot flow can have multiple collectors.
 
## 6. StateFlow and SharedFlow
- With StateFlow and SharedFlow, flows can efficiently update state and update values to multiple consumers.
- Both are hot flows.

   #### StateFlow :
     - When the collector begins collecting,an initial value is emitted.
     - We can create a variable like : val stateFlow = MutableStateFlow(0)
     - The only value that is emitted is the last known value.
     - The value property allows us to check the current value. Without collecting, it keeps a history of one value that can be accessed directly.
     - It does not emit consecutive repeated values. When the value differs from the previous item, it emits the value.
     - Lifecycle awareness of the Android component makes it similar to LiveData. In order to add Lifecycle awareness to it,
       we should use repeatOnLifecycle scope with StateFlow.
       
> Ex: Assume we have StateFlow :
>  val stateFlow = MutableStateFlow(0)
> we start collecting on it:
>
>stateFlow.collect {
>    println(it)
> }       
> As soon as we start collecting, we will get:
> 0
> We get this "0" as it takes an initial value and emits it immediately.
>
> And then, if we keep on setting the values as below:
>
> stateFlow.value = 1
> stateFlow.value = 2
> stateFlow.value = 2
> stateFlow.value = 1
> stateFlow.value = 3
> we will get the following output:
>
>
> 1
> 2
> 1
> 3
> Notice here that we are getting "2" only once, not twice. As it does not emit consecutive repeated values.
> Suppose we add a new collector now:
>
> stateFlow.collect {
>    println(it)
> }
> We will get:
> 3
> As the StateFlow stores the last value and emits it as soon as a new collector starts collecting.
>        
 
  - #### SharedFlow :
     - By default, it does not emit any value since it does not need an initial value.
     - We can create a variable like : val sharedFlow = MutableSharedFlow<Int>()
     - With the replay operator, it is possible to emit many previous values at once.
     - It does not have a value property.
     - The emitter emits all the values without caring about the distinct differences from the previous item. It emits consecutive repeated values also.
     - It is Not similar to LiveData.     
     
> Ex: Assume we have SharedFlow  :
>  val stateFlow = MutableSharedFlow<Int>
>  we start collecting on it:
>
> sharedFlow.collect {
>    println(it)
> }  
> As soon as we start collecting, we will not get anything as it does not take an initial value.
>
> And then, if we keep on emitting the values as below:
>
> sharedFlow.emit(1)
> sharedFlow.emit(2)
> sharedFlow.emit(2)
> sharedFlow.emit(1)
> sharedFlow.emit(3)
> we will get the following output:
>
> 1
> 2
> 2
> 1
> 3  
>
>Notice here that we are getting "2" twice. As it emits consecutive repeated values also.
>
> Suppose we add a new collector now:
>
>
> sharedFlow.collect {
>    println(it)
> }  
> We will not get anything as the SharedFlow does not store the last value.
>
   #### NOTE:
     - StateFlow is a type of SharedFlow. StateFlow is a specialization of SharedFlow.
  
   #### UseCase For StateFlow
     - Get the list of the users from viewModel.
     - If orientation changes, the ViewModel gets retained, and our collector present in the Activity will resubscribe to collect. 
         The List will be collected((StateFlow keeps the last value))
     - Advantage: No need for a new network call.
    
   #### UseCase For SharedFlow
     - Suppose we are doing a task, if that task gets failed, we have to show Snackbar.
      - When our viewModel starts the task and gets failed. It will set the value true to Activity collector will get the value as true and show the Snackbar.
      - If orientation changes, the ViewModel gets retained, and our collector present in the Activity will resubscribe to collect. 
         Nothing will get collected here as SharedFlow does not keep the last value.
         And that is fine. We should not show the Snackbar again on orientation changes.
      - Advantage: It does not show Snackbar again as intended.
  
