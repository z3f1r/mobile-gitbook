# Parse error body

Как парсить ошибки в ответе \(4xx, 5xx\)

```kotlin
val authApi = ControllerApi().getAuthApi()
val refreshTokenReq = authApi.register(user)

refreshTokenReq.enqueue(object : Callback<...>) {
    override fun onResponse(call: ..., response: ...) {
        if (response.isSuccessful) {
            ...
        }
        else {
            val gson = Gson()
            val type = object : TypeToken<MyErrorData>(){}.type
            val myErrorBody: MyErrorData? = gson.fromJson(response.errorBody()!!.charStream(), type)
        }
    }
}
```

