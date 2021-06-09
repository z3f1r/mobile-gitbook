# Hermes

Hermes - JS Engine. Перегоняет jS в байткод и от этого все работае быстрее

[https://facebook.github.io/react-native/docs/hermes](https://github.com/facebook/hermes)  
release бинари: [https://github.com/facebook/hermes/releases](https://github.com/facebook/hermes/releases)

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

## Tools

оф бинари можно найти по ссылке: [https://github.com/facebook/hermes/releases](https://github.com/facebook/hermes/releases)

Туда входят: 

* hbcdump
* hdb
* hermes
* hermesc
* hvm

### hermes

```text
Запуск JS-кода:
$ hermes test.js

Compiling and Executing JavaScript with Bytecode:
$ hermes -emit-binary -out test.hbc test.js
$ hermes test.hbc
```

### hbcdump

[hbcdump](https://github.com/facebook/hermes/releases) — Hermes bytecode disassembler

```text
$ ./hbcdump -objdump-disassemble index.android.bundle
hbcdump> dis

d0310a88a868dfb1ee21d12e9011725b1f716875:     file format HBC-74

Disassembly of section .text:

000000000002ca48 <_0>:
0002ca48:       30 44 08 00 00        DeclareGlobalVar        $0x000844
0002ca4d:       30 48 08 00 00        DeclareGlobalVar        $0x000848
[...]
hbcdump> quit
```

### hdb

JavaScript command line debugger

### hermesc

Standalone Hermes compiler. This can compile JavaScript to Hermes bytecode, but does not support executing it.

### hvm

Standalone Hermes VM. This can execute Hermes bytecode, but does not support compiling it.

### hbctool

[hbctool](https://github.com/bongtrop/hbctool) — инструмент для патчинга \(можно и hex-редактор использовать\)

```text
pip install hbctool

(hack) bongtrop@bongtrop-pc:lab/ $ hbctool disasm HermesReversingLab/assets/index.android.bundle HermesReversingLabHASM
[*] Disassemble 'HermesReversingLab/assets/index.android.bundle' to 'HermesReversingLabHASM' path
[*] Hermes Bytecode [ Source Hash: d0310a88a868dfb1ee21d12e9011725b1f716875, HBC Version: 74 ]
[*] Done

After disassembling HBC (Hermes Bytecode) to HASM (I named it; stands for Hermes Assembly). 
In the HermesReversingLabHASM directory, there are 3 files as follows:

    metadata.json: stores the important information of Hermes bytecode file
    instruction.hasm: stores the application instructions or logics in HASM format (edit application logics in this file)
    string.json: store the application strings or texts (edit strings in this file)

Edit the application’s instruction in HermesReversingLabHASM/instruction.hasm.

Save the file and assemble HASM to the HBC by using hbctool.

(hack) bongtrop@bongtrop-pc:lab/ $ hbctool asm HermesReversingLabHASM HermesReversingLab/assets/index.android.bundle 
[*] Assemble 'HermesReversingLabHASM' to 'HermesReversingLab/assets/index.android.bundle' path
[*] Hermes Bytecode [ Source Hash: d0310a88a868dfb1ee21d12e9011725b1f716875, HBC Version: 74 ]
[*] Done

и далее подписываем apk
```

## \[Frida\] Android

подключение к ядру, исполнить код не смог пока

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

