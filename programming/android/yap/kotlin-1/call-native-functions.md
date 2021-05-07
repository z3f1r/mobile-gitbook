# Call Native Functions

## Вызов нативных функций

### From C++

Пример функции, которую можно вызвать из cpp:

```kotlin
@JvmStatic
fun getSSLCertificatesFile(context: Context): String? {...}
```

Сам вызов:

```cpp
QAndroidJniObject certificate_path = QAndroidJniObject::callStaticObjectMethod(
        "SSLCertificateProvider",
        "getSSLCertificatesFile",
        "(Landroid/content/Context;)Ljava/lang/String;",
        androidContext.object()
);
```

### From Kotlin

Пример функции, которую можно вызвать из kotlin:

```cpp
extern "C" JNIEXPORT jstring JNICALL Java_ru_dgis_example_curl_MainActivity_stringFromJNI(JNIEnv * env, jobject)
{
    const auto s = to_utf16(get_string());
    return env->NewString(reinterpret_cast<const jchar *>(s.c_str()), s.length());
}
```

В Kotlin объявление будет следующим, далее можно будет вызывать функцию stringFromJNI из kotlin:

```kotlin
class Test {

    external fun stringFromJNI(): String

    companion object {
        init {
            System.loadLibrary("native-lib")
        }
    }
}
```

