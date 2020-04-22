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

Еще ссылки:  
[https://medium.com/@nhaarman/a-dive-into-async-await-on-android-5a6699029aa3](https://medium.com/@nhaarman/a-dive-into-async-await-on-android-5a6699029aa3)  
[https://medium.com/@jitesh313/network-calls-kotlin-coroutine-retrofit-2-e058ebc56189](https://medium.com/@jitesh313/network-calls-kotlin-coroutine-retrofit-2-e058ebc56189)



