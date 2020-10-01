# Android: SSL Unpinning

{% embed url="https://habr.com/post/307774/" %}

норм статья про поиски мест где на стороне библиотек системных происходит ssl pinning: [https://xakep.ru/2019/09/12/ssl-sniffing/](https://xakep.ru/2019/09/12/ssl-sniffing/)

Android SSL Pinning via frida/objection  
[https://vavkamil.cz/2019/09/15/how-to-bypass-android-certificate-pinning-and-intercept-ssl-traffic/](https://vavkamil.cz/2019/09/15/how-to-bypass-android-certificate-pinning-and-intercept-ssl-traffic/)

Cordova: [http://ivancevich.me/articles/ignoring-invalid-ssl-certificates-on-cordova-android-ios/](http://ivancevich.me/articles/ignoring-invalid-ssl-certificates-on-cordova-android-ios/)

### Как на новых android \(попробовать\)

В Android, начиная с 7 версии, сертификаты пользователя не подходят для перехвата трафика. Надо их добавлять в хранилище доверенных сертификатов \(как и в Apple, только делается это нетривиально слегка\): [https://blog.ropnop.com/configuring-burp-suite-with-android-nougat/](https://blog.ropnop.com/configuring-burp-suite-with-android-nougat/)

При добавлении сертификата первым способом, может возникнуть проблема, что файловая система находится в режиме read-only. В этом случае:  
mount -o rw,remount /system  
добавляем сертификат  
mount -o ro,remount /system

### Создание сертов, совместимых с системными сертами Android

```text
Имя серта:
openssl x509 -in <some.cer> -subject_hash_old

Составные части:
Серт:
openssl x509 -in <some.cer>

Серт в текстовом виде:
openssl x509 -in <some.cer> -noout -text

Fingerprint:
openssl x509 -in <some.cer> -noout -fingerprint -sha1

Или все разом:
openssl x509 -in FiddlerRoot.cer -inform DER -text -subject_hash_old -sha1 -fingerprint
Главное, потом расположиить в правильном порядке
```

Системные серты здесь: `/system/etc/security/cacerts`

Пользовательские сертификаты хранятся здесь: `/data/misc/user/0/cacerts-added` На более старых устройствах их можно найти здесь: `/data/misc/keychain/cacerts-added`

1. Экспортируем сертификат \(CER\|DER\)
2. openssl x509 -inform DER -in cacert.der -out cacert.pem
3. openssl x509 -inform PEM -subject\_hash\_old -in cacert.pem \|head -1
4. mv cacert.pem .0
5. mv /sdcard/.0 /system/etc/security/cacerts/  

   chmod 644 /system/etc/security/cacerts/.0

6. \*chgrp - на root если вдруг группа не root стоит: chgrp root &lt;file&gt;
7. \*chown root &lt;file&gt; - если вдруг пользователь-владелец не root
8. adb reboot

   В trusted creds появляется наш сертификат

### Перехват через цепочку: android -&gt; mitmproxy -&gt; burp

Запускаем burp: `localhost:<port_burp>`  
Запускаем mitmproxy: `mitmproxy -p <port_mitmproxy> --mode upstream:localhost:<port_burp> --ssl-insecure`  
На телефоне: `set proxy with <port_mitmproxy>`

Из-за чего такие финтеля: не получается поставить серта бурпа на телефоне в системные. Серт mitmproxy становится норм!

### Патчинг на примере Uber

1 Инструменты: JDK, Android SDK \(Tool: zipalign\)  
Добавить их в PATH:  
C:\path\to\jdk\bin  
%USERPROFILE%\AppData\Local\Android\sdk\build-tools\23.0.2

2 Ставим Burp на прослушивание, в WiFi настройках телефона ставим наш прокси. Запускаем приложение - на аутентификации виснет.

3 Что случилось:  
Дело в том, что Burp Suite при перехвате HTTPS-соединений \(а мы помним, что все соединения устройства проксируются через него\) подменяет сертификат веб-сервера на свой, который, естественно, не входит в список доверенных.  
Круто! -&gt; прокидываем наш сертификат на устройство в список доверенных сертификатов.

4 Опять пытаемся войти в свой аккаунт. Сейчас приложение Uber сообщит нам только о неудачной попытке аутентификации – значит прогресс есть, осталось только обойти certificate pinning.

5 Откроем Uber.apk как ZIP-архив. В _/res/raw лежит ssl\_pinning\_certs\_bk146.bks. По его расширению можно понять, что Uber использует хранилище ключей в формате BouncyCastle \(BKS\). Из-за этого нельзя просто заменить сертификат в приложении. Сначала нужно сгенерировать BKS-хранилище. Для этого качаем JAR \(_[_https://www.bouncycastle.org/latest\_releases.html_](https://www.bouncycastle.org/latest_releases.html)_; bcprov-ext-jdk_.jar\) для работы с BKS.  
Пример команды:  
keytool -import -v -trustcacerts -alias mybks -file FiddlerRoot.cer -keystore ssl\_pinning\_certs\_bk146.bks -storetype BKS -provider org.bouncycastle.jce.provider.BouncyCastleProvider -providerpath bcprov-ext-jdk15on-160.jar -storepass spassword

Копируем полученное хранилище в архив \*.apk

6 Теперь надо подписать приложение  
Удаляем из apk папку META-INF со старой подписью приложения и приступаем к генерации новой  
Создаем хранилище ключей и генерируем в нем ключ для подписи apk: keytool -genkey -keystore mykeys.keystore -storepass spassword -alias mykey1 -keypass kpassword1 -dname "CN=ololo O=HackAndroid C=RU" -validity 10000 -sigalg MD5withRSA -keyalg RSA -keysize 1024

Подписываем только что сгенерированным ключом наш APK: jarsigner -sigalg MD5withRSA -digestalg SHA1 -keystore mykeys.keystore -storepass spassword -keypass kpassword1 Uber.apk mykey1

Теперь осталось выровнять данные в архиве по четырехбайтной границе: zipalign -f 4 Uber.apk Uber.apk\_zipal.apk

### Установка сертификатов в iPhone

Для того, чтобы норм перехватывать трафик, необходимо не только в профиль ставить сертификат, но и в доверенные его переводить!

