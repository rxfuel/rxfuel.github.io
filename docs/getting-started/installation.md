# Installation

Add the following dependency in your module build.gradle :

```gradle
implementation 'com.rxfuel.rxfuel:rxfuel-android:0.0.5'
```

Add `jcenter` repository in your project build.gradle : 

```gradle
allprojects {
    repositories {
        jcenter()
    }
}
```
You will also need to add [RxJava2](https://github.com/ReactiveX/RxJava) and [RxAndroid](https://github.com/ReactiveX/RxAndroid) to your project. We also recommend adding [RxBinding](https://github.com/JakeWharton/RxBinding) to get reactive binding for Android UI widgets.