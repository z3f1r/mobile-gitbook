# Common

Про аккаунт для xiaomi девайсов:  
Запускаешь miflash\_unlock.exe   
Входишь под учеткой login/password \(у вас своя учетка, ее легко запросить указывая, что вы ресерчер\): **4150931399**   
номер +7916..23

```text
Загрузиться в recovery (в т.ч. и TWRP):
~# adb reboot recovery

Загрузиться в загрузчик (fastboot):
~# adb reboot bootloader

Очистить раздер с recovery:
~# fastboot erase recovery

Записать наш recovery:
~# fastboot flash recovery recovery.img

Выйти из recovery mod:
~# fastboot reboot

Загрузиться в нашу прошивку (сотрет существующую):
~# fastboot boot recovery.img

Сохранить стоковую recovery:
~# adb shell
~# su
~# dd if=/dev/block/mmcblk0p43 of=/sdcard/recovery.img
Забираем ее к себе на комп

Делаем nondroid backup:
1. Загружаем кастомную recovery (Например, TWRP).
2. Делаем backup


Важно: Если установить TWRP, но не установить SU (SuperSu or Magisk) или Dm-verity and Forced Encryption Disabler (https://build.nethunter.com/android-tools/no-verity-opt-encrypt/)
то телефон зависнет на логотипе и не загрузится.


По сути, своя прошивка ставится в следующей последовательности:
~# fastboot erase recovery
~# fastboot flash recovery twrp.img
~# fastboot boot twrp.img
```

