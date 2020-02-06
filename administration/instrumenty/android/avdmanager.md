---
description: Создание и изменение виртуальных девайсов
---

# avdmanager

Находится по пути $ANDROID\_SDK/tools/bin

```text
Фулл док: https://developer.android.com/studio/command-line/avdmanager

Общий синтаксис:
avdmanager [global options] command [command options]

Global options
-s 	Silent mode: only errors are printed out
-h 	Usage help
-v 	Verbose mode: errors, warnings and informational messages are printed. 

Commands & Command options
1. Create AVD
create avd -n name -k "sdk_id" [-c {path|size}] [-f] [-p path]
    -n - name 
    
    -c {path|size}: The path to the SD card image for this AVD or the size of a new SD card image to create for this AVD, in KB or MB, denoted with K or M. For example, -c path/to/sdcard/ or -c 1000M.
    -f: Force creation of the AVD. Use this option if you need to overwrite an existing AVD with a new AVD using the same name.
    -p path: Path to the location where the directory for this AVD's files will be created. If you do not specify a path, the AVD will be created in ~/.android/avd/.

ex: avdmanager create avd -n test -k "system-images;android-25;google_apis;x86"

2. Delete AVD
delete avd -n name

3. Move Or/And Rename
move avd -n name [-p path] [-r new-name]
-p - куда перенести
-r - новое имя

4. List AVD, Device Definitions, targets
list [target|device|avd] [-c]
-c - для компактного вывода (не работает, если выводить все три категории)
```

Для того, чтобы можно было найти в системе конфиг нашего эмулятора \(файл .avd\), надо в ANDROID\_SDK\_HOME/avd или в ANDROID\_AVD\_HOME или в HOME/.android/avd определить .ini файл подобного вида \(заменив пути так, как надо нам\):

```text
avd.ini.encoding=UTF-8
path=/Users/o.petrakov/.android/avd/Sbol_SberKids_-_API_28_Nexus_5_.avd
path.rel=avd/Sbol_SberKids_-_API_28_Nexus_5_.avd
target=android-28
```

