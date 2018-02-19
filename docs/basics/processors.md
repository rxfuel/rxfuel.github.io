# Processors

> Processor is basically an [Observable transformer](http://reactivex.io/RxJava/javadoc/rx/Observable.Transformer.html) that receives an Action Observable and returns Result Observable.

A processor is used to do a specific task such as api requests, database query, long background processing etc. They are highly decoupled with UI and can be reused. 

A Processor is defined for an action and return Result Observable. It could emit sub result such as loading, failure, etc by using Observable side effect methods.

```kotlin
sealed class MyAction : RxFuelAction {
    data class SearchAction(val query: String) : MyAction()
}
```