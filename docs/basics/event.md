# Event

> Event is basically a class that represents some event that happens from the android device. It could be user UI interactions, GPS location update etc.

Every activity should have a base event class that is inherited from `RxFuelEvent`. Since you will have many events in your Activity you can define a class for each as a sub class.

RxFuelEvent has an abstract Boolean member val `hasProcessor` that you need to implement in your event classes. Set it to `true` if that particular event needs processing such as web request, long background process, database query etc. Such events need to be mapped to action in your ViewModel and define a processor for it.

```kotlin
sealed class MyEvent : RxFuelEvent {
    data class SearchEvent(val query: String, override val hasProcessor: Boolean = true) : MyEvent()
    sealed class ButtonClick : MyEvent() {
        override val hasProcessor: Boolean = false
    }
}
```