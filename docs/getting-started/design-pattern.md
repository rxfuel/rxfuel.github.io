# Design Pattern

> Even though RxFuel is a framework, your project won't be entitled to the boundaries of a framework.

Each Activity is will a have an Observable stream that takes in events and emits view states by which the UI is rendered. Activity is binded with it's ViewModel. Everytime activity is re-created, the previous view states is emitted again to render UI to the latest. This eliminates the need of using fragments to persist the data in an activity lifeycle. If you have any fragment in your activity, you can simply pass the data from view state using an Observable subject and takes in events using another observable subject.

Each activity should have a set of classes to represent `Event`, `Action`, `Result` and `ViewState`.

You can have any structure for your project, but we recommend the following structure.

```
package
|
└───processors
│   │
│   │   processor1.kt
│   │   processor2.kt
│   │   ...
│   
└───ui
│   │   
│   └───activity1
│   │   │   
│   │   └───fragments
│   │   │   │
│   │   │   │  fragment1.kt 
│   │   │   │  ...
│   │   │   activity.kt
│   │   │   activityViewModel.kt
│   │   │   activityEvent.kt
│   │   │   activityAction.kt
│   │   │   activityResult.kt
│   │   │   activityViewState.kt
│   │   ...

       
```