# View

> View is basically the Activity in context. 

Implement `RxFuelView<MyEvent, MyViewState>` in your activity with your `Event` and `ViewState` type as paramters.
You will need to implement `events()` and `render(state: MyViewState)` methods in your activity.

```kotlin
class MyActivity : AppCompatActivity(), RxFuelView<MyEvent, MyViewState> {
    ...
    override fun events(): Observable<MyEvent>? {
        ...
    }

    override fun render(state: MyViewState) {
        ...
    }
}

```

## Bind ViewModel

Create a `RxFuel` instance with your activity context as paramter. Call `bind(viewModel)` with your ViewModel instance as parameter at the bottom `onCreate()` function and `unbind()` in `onDestroy()` function.

```kotlin
class MyActivity : AppCompatActivity(), RxFuelView<MyEvent, MyViewState> {

    private val myViewModel = MyViewModel()
    private val rxFuel = RxFuel(this)
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        ...
        rxFuel.bind(myViewModel)
    }

    override fun events(): Observable<MyEvent>? {
        ...
    }

    override fun render(state: MyViewState) {
        ...
    }
    
    override fun onDestroy() {
        super.onDestroy()
        rxFuel.unbind()
    }

}
```

## Emit Events

Create a RxJava2 Observable binding for all your UI interactions or Android data updates. We recommend using [RxBinding](https://github.com/JakeWharton/RxBinding) library as it provides bindings for all android UI widgets. 
Use [map](http://reactivex.io/documentation/operators/map.html) operator to convert each observable data to an Event.

You can merge all your event Observables using [merge](http://reactivex.io/documentation/operators/merge.html) operator into a single Observable and return them in `events()` function.

```kotlin
class MyActivity : AppCompatActivity(), RxFuelView<MyEvent, MyViewState> {

    private val myViewModel = MyViewModel()
    private val rxFuel = RxFuel(this)
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        ...
        rxFuel.bind(myViewModel)
    }
    
    override fun events(): Observable<MyEvent>? {
        return Observable.merge(
                et_query.textChanges()
                        .debounce(1,TimeUnit.SECONDS, AndroidSchedulers.mainThread())
                        .skip(1)
                        .filter { it.length > 3 }
                        .map { query -> MyEvent.Search(query.toString()) },
                btn.clicks()
                        .map { MyEvent.ButtonClick }
                )
    }

    override fun render(state: MyViewState) {
        ...
    }
    
    override fun onDestroy() {
        super.onDestroy()
        rxFuel.unbind()
    }
}
```

## Render UI

`render(state: MyViewState)` will be called with latest ViewState everytime there is ViewState update or activity is recreated. 

Simply update your UI elements with the data from in the ViewState provided in your render function.

```kotlin
class MyActivity : AppCompatActivity(), RxFuelView<MyEvent, MyViewState> {

    private val myViewModel = MyViewModel()
    private val rxFuel = RxFuel(this)
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        ...
        rxFuel.bind(myViewModel)
    }
    
    override fun events(): Observable<MyEvent>? {
        return Observable.merge(
                et_query.textChanges()
                        .debounce(1,TimeUnit.SECONDS, AndroidSchedulers.mainThread())
                        .skip(1)
                        .filter { it.length > 3 }
                        .map { query -> MyEvent.Search(query.toString()) },
                btn.clicks()
                        .map { MyEvent.ButtonClick }
                )
    }

    override fun render(state: MyViewState) {
        progressBar.visibility = if(state.loading) VISIBLE else GONE
        if(!state.loading) 
            listSubject.onNext(state.response)
        if(state.errorMessage!=null) 
            Toast.makeText(this,state.errorMessage,LENGTH_LONG).show()
    }
    
    override fun onDestroy() {
        super.onDestroy()
        rxFuel.unbind()
    }
}
```
