# ControllerApi

```kotlin
package com.zfr.network.network

import com.jakewharton.retrofit2.adapter.kotlin.coroutines.CoroutineCallAdapterFactory
import com.zfr.network.network.interfaces.HttpApi
import okhttp3.CertificatePinner
import okhttp3.OkHttpClient
import okhttp3.Protocol
import okhttp3.logging.HttpLoggingInterceptor
import retrofit2.Retrofit
import java.util.*
import java.util.concurrent.TimeUnit

class ControllerApi {
    val BASE_URL: String = "https://192.168.1.65:8080/"

    private val loggingInterceptor =  HttpLoggingInterceptor().apply {
        level = HttpLoggingInterceptor.Level.BODY
    }

    fun config_http_client(http2_support: Boolean = false): OkHttpClient.Builder {

        val client = OkHttpClient().newBuilder()

        client.readTimeout(30, TimeUnit.SECONDS)
        client.writeTimeout(30, TimeUnit.SECONDS)
        client.connectTimeout(30, TimeUnit.SECONDS)
        client.addInterceptor(loggingInterceptor)

        if (http2_support) {
            client.protocols(Arrays.asList(Protocol.HTTP_2))
        }
        return client
    }

    fun getHttpApi(host: String, port: Int, sslPinningEnabled: Boolean, http2_support: Boolean): HttpApi {
        lateinit var retrofit: Retrofit

        val httpClient = config_http_client(http2_support)

        if (sslPinningEnabled) {
            httpClient.certificatePinner(
                CertificatePinner.Builder()
                    .add(host, "sha256/3MnSPmEDhbuEDGiiUeg0oMw0m92SKIVvp8Aruhx4N0M=")
                    .build()
            )
        }
        retrofit = Retrofit.Builder()
            .client(httpClient.build())
            .baseUrl("https://${host}:${port}/")
            .addCallAdapterFactory(CoroutineCallAdapterFactory())
            .build()

        return retrofit.create(HttpApi::class.java)
    }

}
```

