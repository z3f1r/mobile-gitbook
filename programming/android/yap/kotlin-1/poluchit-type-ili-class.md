# Получить Type или Class

```kotlin
val type = object : TypeToken<MyErrorData>(){}.type
val klass = MyErrorData::class
где-то еще такое видел:
val klass = MyErrorData::class.java
val klass = MyErrorData.class
```

