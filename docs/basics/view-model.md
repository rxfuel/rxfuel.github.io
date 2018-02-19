# ViewModel

> ViewModel connects UI layer with Domain layer

Every activity should have a ViewModel. Define your ViewModel class implementing `RxFielViewModel<MyEvent, MyViewState>` with your Event and ViewState as type parameters.

Your ViewModel must implement `eventToViewState`, `eventToAction` and `resultToViewState` functions and `idleState` value.

```kotlin
class MyViewModel : RxFuelViewModel<MyEvent, MyViewState> {
    
    val initialState = ...
    
    fun eventToAction(event : MyEvent) : RxFuelAction {
        ...
    }
    
    fun eventToViewState(previousState : MyViewState, event : MyEvent) : MyViewState {
        ...
    }
    
    fun resultToViewState(previousState : MyViewState, result : RxFuelResult) : MyViewState {
        ...
    }
}

```

## initialState

`initialState` will be the emitted when your activity starts for the first time. Set it's value with some initial ViewState. 

```kotlin
class MyViewModel : RxFuelViewModel<MyEvent, MyViewState> {
    
    val initialState = MyViewState(false, null, null, null)
    
    fun eventToAction(event : MyEvent) : RxFuelAction {
        ...
    }
    
    fun eventToViewState(previousState : MyViewState, event : MyEvent) : MyViewState {
        ...
    }
    
    fun resultToViewState(previousState : MyViewState, result : RxFuelResult) : MyViewState {
        ...
    }
}
```

## eventToAction

This function is used to map your Domain scopped Events to Action and to further trigger the processor.

```kotlin
class MyViewModel : RxFuelViewModel<MyEvent, MyViewState> {
    
    val initialState = MyViewState(false, null, null, null)
    
    fun eventToAction(event : MyEvent) : RxFuelAction {
        return when(event) {
            is MyEvent.Search -> ApiAction.Seacrh(event.query)
            else -> // provide some fake action as this case will not be reached.
        }
    }
    
    fun eventToViewState(previousState : MyViewState, event : MyEvent) : MyViewState {
        ...
    }
    
    fun resultToViewState(previousState : MyViewState, result : RxFuelResult) : MyViewState {
        ...
    }
}
```

## eventToViewState

This function is used to map your UI scopped Events to ViewState.

```kotlin
class MyViewModel : RxFuelViewModel<MyEvent, MyViewState> {
    
    val initialState = MyViewState(false, null, null, null)
    
    fun eventToAction(event : MyEvent) : RxFuelAction {
        return when(event) {
            is MyEvent.Search -> ApiAction.Seacrh(event.query)
            else -> // provide some fake action as this case will not be reached.
        }
    }
    
    fun eventToViewState(previousState : MyViewState, event : MyEvent) : MyViewState {
        return when(event) {
            MyEvent.ButtonClick -> previousState.copy(buttonClicked = true)
            else -> MyViewState(false, null, null, null)
        }
    }
    
    fun resultToViewState(previousState : MyViewState, result : RxFuelResult) : MyViewState {
        ...
    }
}
```

## resultToViewState

This function is used to map processor Result to ViewState.

```kotlin
class MyViewModel : RxFuelViewModel<MyEvent, MyViewState> {
    
    val initialState = MyViewState(false, null, null, null)
    
    fun eventToAction(event : MyEvent) : RxFuelAction {
        return when(event) {
            is MyEvent.Search -> ApiAction.Seacrh(event.query)
            else -> // provide some fake action as this case will not be reached.
        }
    }
    
    fun eventToViewState(previousState : MyViewState, event : MyEvent) : MyViewState {
        return when(event) {
            MyEvent.ButtonClick -> previousState.copy(buttonClicked = true)
            else -> MyViewState(false, null, null, null)
        }
    }
    
    fun resultToViewState(previousState : MyViewState, result : RxFuelResult) : MyViewState {
        return when(result) {
            is ApiResult.Search.InFlight  -> 
                    previousState.copy(isLoading = true)
            is ApiResult.Search.Success -> 
                    previousState.copy(isLoading = false, response = result.response)
            is ApiResult.Search.Failure -> 
                    previousState.copy(isLoading = false, errorMessage = result.errorMessage)
            else -> MyViewState(false, null, null, null)
        }
    }
}
```
