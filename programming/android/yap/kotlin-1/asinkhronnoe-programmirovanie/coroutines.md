# Coroutines

Ссылки:  
[https://android.jlelse.eu/kotlin-coroutines-and-retrofit-e0702d0b8e8f](https://android.jlelse.eu/kotlin-coroutines-and-retrofit-e0702d0b8e8f)

В app/build.gradle:

```text
// Kotlin & Coroutines
implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:$coroutines_version"
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:$coroutines_version"
```

В коде

```kotlin
Thread(Runnable{
    // your code
}).start()
```

