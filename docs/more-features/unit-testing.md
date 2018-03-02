# Unit Testing

High test coverage is one of the major advantage of RxFuel. You can simulate all events in your activity by emitting the relevent events.
`TestRxFuelViewModel` makes it easier to write unit tests.

```kotlin
class MyTest {

    private lateinit var testRxFuel: TestRxFuelViewModel<MyEvent, MyViewState>
    private lateinit var testObserver: TestObserver<MyViewState>
    
    @Before
    fun setup() {
    
        testRxFuel = TestRxFuelViewModel(MyViewModel())
                .withProcessorModules(MyProcessorModule())
                .synchronously()

        testObserver = testRxFuel.getObserver()
    }
    
    
    @Test
    fun test() {
        testRxFuel.events(
                MyEvent.Search("retrofit"),
                MyEvent.ButtonClick
        )

        testObserver.assertValueAt(0, { state ->
            state == MyViewState.idle()
        })

        testObserver.assertValueAt(1, { state ->
            state == MyViewState(
                    true,
                    listOf(),
                    null,
                    null,
                    true,
                    null)
        })

        testObserver.assertNoErrors()
    }

    @After
    fun after(){
        testRxFuel.destroy()
    }
    
}
    
```