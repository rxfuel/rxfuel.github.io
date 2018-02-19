# ViewState

> ViewState represents the data that is needed to render the UI.

Every Activity should have a ViewState [data class](https://kotlinlang.org/docs/reference/data-classes.html) that is inherited from `RxFuelViewState`. Define all variable that need to render the UI. 

ViewStates instances are created using Result/Event with a copy of previous ViewState instance from your ViewModel.

!> `RxFuelViewState` have an abstract val `navigate`. Assign it with an Activity class if you need to navigate and `null` otherwise. Keep it `null` by default.

```kotlin
data class MyViewState(
                        val isLoading : Boolean,
                        val response: List<Item>? = null,
                        val errorMessage : String? = null,
                        override var navigate: KClass<out FragmentActivity>? = null
                      ) : RxFuelViewState
```