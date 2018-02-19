# Event

> Event represents an interaction that happened from the android device. It could be user UI interaction, GPS location update etc.

Every activity should have a base event [sealed class](https://kotlinlang.org/docs/reference/sealed-classes.html) that is inherited from `RxFuelEvent`. Since you will have many events in your Activity, you can define a class for each as a sub class in a nested way. You can make use [data class](https://kotlinlang.org/docs/reference/data-classes.html) to easily represent your event data.

```kotlin
sealed class MyEvent : RxFuelEvent {

    data class Search(val query: String) : MyEvent()
    
    object ButtonClick : MyEvent()
    
}
```

## Event Scopes

Events need to be categorized by their usage. All event might not require a processor as it could just be a view state update. Such event's scope is UI level and need to annotate them with `@EventScope(Scope.UI)`. Event that require processing have domain scope and need to annotate them with `@EventScope(Scope.DOMAIN)`.

```kotlin
sealed class MyEvent : RxFuelEvent {

    @EventScope(Scope.DOMAIN)
    data class Search(val query: String) : MyEvent()
    
    @EventScope(Scope.UI)
    object ButtonClick : MyEvent()
    
}
```