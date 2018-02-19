# Result

> Result is a class that represents the data that need to be displayed to the user.

Every Activity should have a base result class that is inherited from `RxFuelResult`. Define a result class for every processor you have as a sub class. In most cases you will have multiple result for a single processor such success result, loading, failure etc. Create a sub class for each in a nested way to make it more idiomatic.

```kotlin
sealed class MyResult : RxFuelAction {
    sealed class SearchResult : MyResult() {
        data class Success(val response : List<Item>) : SearchResult()
        object InFlight : SearchResult()
        data class Failure(val errorMessage : String) : SearchResult()
    }
}
```