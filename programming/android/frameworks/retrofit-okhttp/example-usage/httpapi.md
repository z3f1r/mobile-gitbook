# HttpApi

```kotlin
package com.zfr.network.network.interfaces

import okhttp3.OkHttpClient
import okhttp3.ResponseBody
import okhttp3.logging.HttpLoggingInterceptor
import retrofit2.Call
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import retrofit2.http.GET
import retrofit2.http.Query
import java.util.*
import retrofit2.await
import java.util.concurrent.TimeUnit

interface HttpApi {

    @GET("/api/orders")
    fun request(): Call<ResponseBody>
}
```

