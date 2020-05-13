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

Извлечение бэкапа - Android Backup Extractor

src: [https://github.com/nelenkov/android-backup-extractor](https://github.com/nelenkov/android-backup-extractor)

лезем в /build/libs

```text
java -jar abe-all.jar unpack backup.ab backup.tar [password]
```



