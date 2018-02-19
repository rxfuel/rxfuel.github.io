# Action

> Action represents the data that need to be processed.

Define an Action [sealed class](https://kotlinlang.org/docs/reference/sealed-classes.html) for every processor module implementing `RxFuelAction`. Define different Action classes for every processor it holds as a sub class. 

```kotlin
sealed class MyAction : RxFuelAction {
    data class SearchAction(val query: String) : MyAction()
}
```