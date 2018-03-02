# Navigation

You can navigate to another activity with an event triggered in the destination activity. Use `navigateTo` function with first parameter as KClass of the destination activity and second paramter as an event of destination activity.

```kotlin
rxFuel.navigateTo(
        DestinationActivity::class,
        Event.SomeIntialEvent(someValue)
)
```