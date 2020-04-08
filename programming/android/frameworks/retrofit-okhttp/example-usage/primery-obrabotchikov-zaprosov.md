# Примеры обработчиков запросов

Через классические onResponse/onFailure

```kotlin
 val authApi = ControllerApi().getAuthApi()
 val refreshTokenReq = authApi.register(user)
 
 refreshTokenReq.enqueue(object : Callback<SomeReturnData>) {
     override fun onResponse(...) {
         if (response.isSuccessful) {
             // 2xx, 3xx
         }
         else {
             // 4xx, 5xx
         }
     }
     override fun onFailure(...) {
         // non http, network errors
     }
 }
```



