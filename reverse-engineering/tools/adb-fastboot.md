# adb/fastboot

### Install

{% embed url="https://developer.android.com/studio/releases/platform-tools.html" %}

### Usage

Подробно, как пользоваться ADB:  
[https://gist.github.com/Pulimet/5013acf2cd5b28e55036c82c91bd56d8](https://gist.github.com/Pulimet/5013acf2cd5b28e55036c82c91bd56d8)

Флаги permissions:  
[https://android.googlesource.com/platform/frameworks/base/+/483d785b4239f5f50e2f72292faa7a65b51160dc/docs/html/tools/help/adb.jd](https://android.googlesource.com/platform/frameworks/base/+/483d785b4239f5f50e2f72292faa7a65b51160dc/docs/html/tools/help/adb.jd)

### ADB Man

```text
ADB man
am - Activity Manager

Старт активити
am start [options] intent
-D - debugging
-W - дождаться, пока запустится
-S - остановить приложение перед запуском

Старт сервисов
am startservice [options] intent

Оставить все, что связано с пакетом
adb shell am force-stop package
adb shell am kill [options] package

Закрыть все процессы в background
adb shell am kill-all

broadcast intent
adb shell am broadcast [options] intent

Какой-то мониторинг
adb shell am instrument [options] component
-e name value - set arguments
-p file - write profileng data to file
-w - wait пока не закончится

Start profiler on process, write results to file.
adb shell am profile start process file

Stop profiler on process
adb shell am profile stop process

dump the heap of process, write to file:
adb shell am dumpheap [options] process file

adb shell am to-uri intent
adb shell am to-intent-uri intent


pm - Package Manager
adb shell pm dump <package> - тайминги работы приложения, что запускало и тп

Удалить пользователя (читай: приложение)
adb shell pm remove-user USER_ID
```

![](../../.gitbook/assets/izobrazhenie%20%289%29.png)

### Trubleshooting

Ошибка `adb: failed to install <some>.apk Failure [INSTALL_FAILED_VERSION_DOWNGRADE]`   
Решение: удалить `/data/app/some`

