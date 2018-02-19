# Result

> Result are the outputs of a processor.

Every processor module should have a base result [sealed class](https://kotlinlang.org/docs/reference/sealed-classes.html) that is inherited from `RxFuelResult`. Define a result class for every processor you have as a sub class. In most cases you will have multiple result for a single processor such success result, loading, failure etc. Create a sub class for each in a nested fashion to make it more idiomatic.

```kotlin
sealed class MyResult : RxFuelResult {
    sealed class Search : MyResult() {
        data class Success(val response : List<Item>) : Search()
        object InFlight : Search()
        data class Failure(val errorMessage : String) : Search()
    }
}
```