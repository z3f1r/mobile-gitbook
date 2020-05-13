# Создание и восстановление резервной копии

```text
-shared - для sdcard
-apk - что б еще и apk дампить
-obb - кэш
-nosystem - без системных приложений
-all - все данные приложений
adb backup -nosystem -all -f kitchen.ab

Восстановление: adb backup restore
```

