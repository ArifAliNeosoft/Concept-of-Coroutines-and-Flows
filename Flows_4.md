## 7.callbackFlow
- Many libraries in our Android Project that provides the callback concept to use instead of the Flow API concept.
- callbackFlow used to convert any Callback to Flow API in Kotlin.

> Assume that we can use LocationManager and listen to the changes in location as below:

> //create a location listener
> val locationListener = object : LocationListener {
>    
>    override fun onLocationUpdate(location: Location) {
>        // do something with the updated location
>    }
>
> }
>  
> // register for location updates
> LocationManager.registerForLocation(locationListener)
> 
> // unregister in onDestroy()
> LocationManager.unregisterForLocation(locationListener)

- Here, we have a LocationListener through which we get the onLocationUpdate callback.

- Now, we want to use this LocationManager in the Flow API way.

- Let's do this by creating a function which returns Flow<Location> as below:

> fun getLocationFlow(): Flow<Location> {
>    return callbackFlow {
> 
>        val locationListener = object : LocationListener {
> 
>            override fun onLocationUpdate(location: Location) {
>                 trySend(location)
>            }
>
>        }
>
>        LocationManager.registerForLocation(locationListener)
>
>        awaitClose {
>            LocationManager.unregisterForLocation(locationListener)
>        }
> 
>    }
> }

- Now, it's time to understand what we have done to convert the Callback to Flow API.

- We have followed the following steps to convert the Callback to Flow API in Kotlin:

  - Create a function to return the Flow<Location>.
  - Use callbackFlow as the return block.
  - Use trySend() for the location updates.
  - Use awaitClose block to unregister.
- Now, we can use the above function as below:


> launch {
>    getLocationFlow()
>    .collect { location ->
>        // do something with the updated location
>    }
> }
