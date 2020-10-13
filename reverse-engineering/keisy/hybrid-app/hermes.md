# Hermes

Hermes - JS Engine. Перегоняет jS в байткод и от этого все работае быстрее

[https://facebook.github.io/react-native/docs/hermes](https://github.com/facebook/hermes)

О подключении в React Native проект [https://facebook.github.io/react-native/docs/hermes](https://facebook.github.io/react-native/docs/hermes)

Отличие от React Native в том, что index.android.bundle будет сериализован \(бинарщина\)

```text
$ hermes -b --dump-bytecode index.android.bundle
Error deserializing bytecode: Wrong bytecode version. Expected 74 but got 62%
$ hermes -version
74

Есть несколько релизов hermes:
v0.4.0 - 74
v0.3.* - 72
v0.2.1 - 62
```

## Android, подключение к ядру, исполнить код не смог пока

```javascript

console.log("[+] Start");
Java.perform(function() {
    try {
        var ReactInstanceManagerHolder = Java.use('org.jitsi.meet.sdk.ReactInstanceManagerHolder');
        console.log("[+] ReactInstanceManagerHolder Found");
        var reactInstanceManager = ReactInstanceManagerHolder.getReactInstanceManager();  // com.facebook.react.h
        var reactContext = reactInstanceManager.i(); // com.facebook.react.bridge.ReactContext
        console.log("[+] Get reactContext");
        console.log("[+] test: " + reactContext.hasCurrentActivity());
    } catch (err) {
        console.log("[-] ReactInstanceManagerHolder Not Found")
    }
});
```

Соответствующий код в приложении

```java
import com.facebook.hermes.reactexecutor.HermesExecutorFactory;
import com.facebook.react.ReactInstanceManager;

// Use the Hermes JavaScript engine.
HermesExecutorFactory jsFactory = new HermesExecutorFactory();

reactInstanceManager
    = ReactInstanceManager.builder()
        .setApplication(activity.getApplication())
        .setCurrentActivity(activity)
        .setBundleAssetName("index.android.bundle")
        .setJSMainModulePath("index.android")
        .setJavaScriptExecutorFactory(jsFactory)
        .addPackages(packages)
        .setUseDeveloperSupport(BuildConfig.DEBUG)
        .setInitialLifecycleState(LifecycleState.RESUMED)
        .build();
```

Это была попытка добраться до jsFactory, однако, все равно не смог найти методы, чтобы что-то исполнить в этом контексте. Вариант далее - разобраться как работает React Debug, и попробовать что-то такое сделать из frida.

