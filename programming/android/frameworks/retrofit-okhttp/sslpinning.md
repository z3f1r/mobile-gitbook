# SSLPinning

SSLPinning \(делается через OkHTTP: сначала проверяем серт через okhttp, потом делаем запрос через ретрофит\)  
[https://proandroiddev.com/configuring-retrofit-2-client-in-android-130455eaccbd](https://proandroiddev.com/configuring-retrofit-2-client-in-android-130455eaccbd)

OkHTTP Client SSL Pinning ex:  
[https://www.codota.com/code/java/methods/okhttp3.OkHttpClient$Builder/sslSocketFactory](https://www.codota.com/code/java/methods/okhttp3.OkHttpClient$Builder/sslSocketFactory)

С добавлением SSL Pinning'а есть нюансы. Если сертификат сервера подписан у доверенного CA, то пиннинг в приложении делается очень просто:

```kotlin
val client = OkHttpClient().newBuilder()
httpClient.certificatePinner(
    CertificatePinner.Builder()
        .add("publicobject.com", "sha256//r8udi/Mxd6pLO7y7hZyUMWq8YnFnIWXCqeHsTDRqy8=")
        .build()
)
...
```

Однако, в случае самоподписанного сертификата сервера, TLS соединение не будет установлено: будет ошибка "trust anchor for certification path not found"

В этом случае надо добавлять свой TrustManager и настраивать sslSocketFactory с сертом сервера.

Пример Trust Manager с полным доверием всем сертам \(в этом случае sslpinning как в примере выше не сработает \(ведь мы всем сертам доверяем\)\)

```kotlin
val trustAllCerts = arrayOf<TrustManager>(object : X509TrustManager {
    @Throws(CertificateException::class)
    override fun checkClientTrusted(chain: Array<java.security.cert.X509Certificate>, authType: String) {

    }

    @Throws(CertificateException::class)
    override fun checkServerTrusted(chain: Array<java.security.cert.X509Certificate>, authType: String) {

    }

    override fun getAcceptedIssuers(): Array<java.security.cert.X509Certificate> {
        return arrayOf()
    }
})

val client = OkHttpClient().newBuilder()
val sslContext = SSLContext.getInstance("TLS")
sslContext.init(null, trustAllCerts, null)
val sslSocketFactory = sslContext.socketFactory
client.sslSocketFactory(sslSocketFactory, trustAllCerts[0] as X509TrustManager)
client.hostnameVerifier(HostnameVerifier {_, _ -> true})
```

