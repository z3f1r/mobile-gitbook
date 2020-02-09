---
description: Инструмент для запуска AVD
---

# emulator

Находится по пути: $ANDROID\_SDK/tools/

```text
Общий синтаксис:
emulator -avd avd_name [ {-option [value]} … ]

default: emulator -avd avd_name -netdelay none -netspeed full

Help по всем командам: 
emulator -help
emulator -help-<command>

Список AVD:
emulator -list-avds

Основные опции:
1. Quick Boot
-no-snapshot-load - загрузить не из снапшота, при выходе делается снапшот
-no-snapshot-save - загрузить из последнего снапшота, но при выходе снапшот не делать
-no-snapshot - выключает фичу со снапшотами => никакого Quick Boot
2. Camera - см в документации
3. Disk Images and Memory
-memory <size> - Выдать эмулятору от 128 до 4096 MBs. Это значение переопределяет настройки AVD.
-sdcard filepath - mount sdcard.img (по дефолту, назодится в директории с данными)
-wipe-data - удалить всю user data (все установленные приложения, все их данные; равносильно откату userdata-qemu.img.
4. Debug
Можно писать всякую дебажную херню и все связанное с логированием (при этом отфильтровывая инфу)
-show-kernel - вывести сообщения ядра. Использовать хотя б раз для проверки, что процесс загрузки работает корректно
5. Network
-port <port> - один порт для консоли и adb
-ports <console-port>,<adb-port> - разные порты для консоли и adb
-tcpdump <filepath> - дамп всего трафика в .cap-файл
6. System
-accel <mode> - работает только для x86,x86_64 образов. По умолчанию включена
...
7. UI
-no-boot-anim - отключить анимацию во время запуска: ускоряет общий процесс

Advanced options:
-no-window - отключить изображение
-shell - дать root-shell
-snapshot <name> - имя для снапшота, с которого осуществляется загрузка и сохранение
-snapshot-list - список доступных снапшотов контейнера
-snapstorage <filepath> - образ, в котором хранятся снапшоты
-writable-system - сделать разделы системы writable

```

После запуска эмулятора, будет доступна консоль для управления им. Пример подключения: `telnet localhost 5554`

**Note:** The emulator listens for connections on ports 5554 to 5585 and accepts connections from localhost only.

Инфа по командам, которыми можно управлять из консоли: [https://developer.android.com/studio/run/emulator-console.html](https://developer.android.com/studio/run/emulator-console.html)

After the console displays `OK`, enter the `auth auth_token` command. Before you can enter [console commands](https://developer.android.com/studio/run/emulator-console.html#querycontrol), the emulator console requires authentication. `auth_token` must match the contents of the `.emulator_console_auth_token` file in your home directory.

If that file doesn't exist, the `telnet localhost console-port` command creates the file, which contains a randomly generated authentication token. To disable authentication, delete the token from the `.emulator_console_auth_token` file or create an empty file if it doesn't exist.





