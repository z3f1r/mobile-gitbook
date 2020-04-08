---
description: Это часть корутинов
---

# Async/Await

Введение: [https://habr.com/ru/post/314656/](https://habr.com/ru/post/314656/)

Концептуальный пример:

```kotlin
fun fun1() = async<String> {
    val user = await(repo.getUser())
    user.username
}
```

