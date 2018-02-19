# Action

> Action is a class that represents the data that need to be processed.

Action class need to be inherited from `RxFuelAction`. We recommend to define it Define an action class for every processor you have as a sub class. You will need to map events that have processor to the action you define, in your ViewModel.

```kotlin
sealed class MyAction : RxFuelAction {
    data class SearchAction(val query: String) : MyAction()
}
```