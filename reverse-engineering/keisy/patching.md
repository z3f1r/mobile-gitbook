# Патчинг

### Android

1 Распаковываем наш apk  
apktool d -r -b my.apk - декомпилим наш apk в smali;  
 -r - не распаковывать ресурсы  
 -b - не вставлять отладочную информацию в smali  
 -s - не дизасмить dex

2 Изменяем, как надо smali, manifest и т.п.

3 Запаковываем в apk  
apktool b my -o my\_pathed.apk  
 -f - форсированная сборка без проверки изменений

4 Подписываем apk  
Удаляем из apk папку META-INF со старой подписью приложения \(если есть\) и приступаем к генерации новой.  
Создаем хранилище ключей и генерируем в нем ключ для подписи apk: `keytool -genkey -keystore mykeys.keystore -storepass spassword -alias mykey1 -keypass kpassword1 -dname "CN=ololo O=HackAndroid C=RU" -validity 10000 -sigalg SHA256withRSA -keyalg RSA -keysize 2048`

Подписываем только что сгенерированным ключом наш APK: `jarsigner -sigalg SHA256withRSA -digestalg SHA1 -keystore mykeys.keystore -storepass spassword -keypass kpassword1 my_pathed.apk mykey1`

Теперь осталось выровнять данные в архиве по четырехбайтной границе: `zipalign -f 4 my_pathed.apk my_pathed_zipal.apk`

Другой вариант подписи:  
Сначала zipalign, затем apksigner \(из android sdk \(build-tools/\[version\]/apksigner.bat sign --ks keystore my.apk\)\)

jarsigner и keytool - часть JDK \(см bin\)  
zipalign - часть Android SDK / build tools

Caution: You must use zipalign at one of two specific points in the app-building process, depending on which app-signing tool you use:  
 - If you use apksigner, zipalign must only be performed before the APK file has been signed. If you sign your APK using apksigner and make further changes to the APK, its signature is invalidated.  
 - If you use jarsigner, zipalign must only be performed after the APK file has been signed.

### iOS



