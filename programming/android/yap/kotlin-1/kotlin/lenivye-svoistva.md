---
description: Будут вычислены при обращении ?
---

# Ленивые свойства



```kotlin
// Ленивые свойства
    val p: String by lazy {
        // compute the string
        return@lazy "sdfsdf"
    }

```

