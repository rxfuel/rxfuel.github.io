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

    override fun stateAfterNavigation(navigationState: MyViewState): MyViewState {
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

    override fun stateAfterNavigation(navigationState: MyViewState): MyViewState {
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
            else -> RxFuelAction.NO_ACTION
        }
    }

    fun eventToViewState(previousState : MyViewState, event : MyEvent) : MyViewState {
        ...
    }

    fun resultToViewState(previousState : MyViewState, result : RxFuelResult) : MyViewState {
        ...
    }

    override fun stateAfterNavigation(navigationState: MyViewState): MyViewState {
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
            else -> RxFuelAction.NO_ACTION
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

    override fun stateAfterNavigation(navigationState: MyViewState): MyViewState {
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
            else -> RxFuelAction.NO_ACTION
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

    override fun stateAfterNavigation(navigationState: MyViewState): MyViewState {
        ...
    }
}
```

## stateAfterNavigation

Evertime an activity is rendered with a view state in which `navigate` is not null, it need to be rendered with a new view state where `navigate` is null.
Otherwise, when the activity is recreated, it will navigate to another activity which is a wrong behaviour.

In some cases, you will also need to change the view before navigating so as to make it ready when the activity is visited again such as hiding loading state etc.

`stateAfterNavigation` is invoked after a navigate state. Return a new state where `navigate` is null and any other state changes if required.

```kotlin
class MyViewModel : RxFuelViewModel<MyEvent, MyViewState> {

    val initialState = MyViewState(false, null, null, null)

    fun eventToAction(event : MyEvent) : RxFuelAction {
        return when(event) {
            is MyEvent.Search -> ApiAction.Seacrh(event.query)
            else -> RxFuelAction.NO_ACTION
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

    override fun stateAfterNavigation(navigationState: MyViewState): MyViewState {
        return navigationState.copy(navigate = null)
    }
}
```
