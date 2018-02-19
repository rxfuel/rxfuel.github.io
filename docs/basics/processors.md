# Processors

> Processor is basically an [Observable transformer](http://reactivex.io/RxJava/javadoc/rx/Observable.Transformer.html) that receives an Action Observable and returns Result Observable.

A processor is used to do a specific task such as api requests, database query, long background processing etc. They are highly decoupled with UI and can be reused. 

A processor is invoked by the ViewModel using Action. You need to specify the Action class for each processor using `@RxFuelProcessor(MyAction.Search::class)` annotation. 

Processor returns Result Observable. It could emit sub result such as loading, failure, etc by using Observable side effect methods.

```kotlin
@RxFuelProcessor(MyAction.Search::class)
val searchProcessor =
        ObservableTransformer<MyAction.Search, ApiResult.Search> { actions ->
            actions.flatMap { action ->
                apiService.getList(action.query)
                        .subscribeOn(Schedulers.io())
                        .map { response -> ApiResult.Search.Success(response.items) }
                        .onErrorReturn { t -> ApiResult.Search.Failure(t.toString()) }
                        .observeOn(AndroidSchedulers.mainThread())
                        .startWith(ApiResult.Search.InFlight)
                }
            }
```

## Processor Module

A processor module is a class/object that contains your processors. It is used to group your processors as a module such as ApiModule, DatabaseModule etc. Each processor module class need to implement `RxFuelProcessorModule`.

```kotlin
object ApiModule : RxFuelProcessorModule {

    @RxFuelProcessor(MyAction.Search::class)
    val searchProcessor = ...
            
    @RxFuelProcessor(MyAction.Comment::class)
    val commentProcessor = ...
}

```

You also need to register all your processor modules with RxFuel from your `Application` class `onCreate()` function. Provide all your processor module as a varang paramater using `RxFuel.registerProcessorModule()` function.

```kotlin

class MyApplication : Application {
    
    override fun onCreate() {
        super.onCreate()
        RxFuel.registerProcessorModule(ApiModule, DatabaseModule)
    }
    
}

```

