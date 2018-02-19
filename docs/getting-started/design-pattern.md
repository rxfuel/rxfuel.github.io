# Design Pattern

> Even though RxFuel is a framework, your project won't be entitled to the boundaries of a framework.

Each Activity have an Observable stream that takes in events and emits view states by which the UI is rendered. UI and business logic are highly decoupled.

An Activity is binded with it's ViewModel. Everytime activity is re-created, the previous view state is re-emitted to render the UI to the latest. This eliminates the need of using fragments to persist the data in an activity lifeycle. If you have any fragment in your activity, you can simply pass the data from view state using an Observable subject and takes in events using another observable subject.

Each activity should have a `Event` and `ViewState` classes to represent the data.

You can have any structure for your project, but we recommend the following structure.

```
package
└───data
│   └───processor1
│   │   │   Processor1Action.kt
│   │   │   Processor1Module.kt
│   │   │   Processor1Result.kt
│   └───processor2
│   │   │   Processor2Action.kt
│   │   │   Processor2Module.kt
│   │   │   Processor2Result.kt
│   │   ...
└───ui
│   └───my
│   │   └───fragments
│   │   │   │  Fragment1.kt 
│   │   │   │  ...
│   │   │   MyActivity.kt
│   │   │   MyViewModel.kt
│   │   │   MyEvent.kt
│   │   │   MyViewState.kt
│   │   │   ...
│   │   ...
│   ...
```