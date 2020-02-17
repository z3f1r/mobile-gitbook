# Services

IntentService - запускается в отдельном потоке \(=&gt; позволяет делать сетевые запросы\)

Service - работает в главном потоке

### Intro

Используются для обработки информации в background Как BroadcastReceivers и Activities, Services могут быть вызваны внешними приложениями =&gt; должны быть также защищены permissions и флагами export.

### Example

Объявление:

```markup
<permission android:name="com.example.mypermission" 
            android:label="my_permission" 
            android:protectionLevel="dangerous">
</permission>
<service android:name="com.example.MyService" 
         android:permission="com.example.mypermission">
    <intent-filter>
         <action android:name="com.example.MY_ACTION" />
    </intent-filter>
</service>
```

 Ex: [https://startandroid.ru/ru/uroki/vse-uroki-spiskom/157-urok-92-service-prostoj-primer.html](https://startandroid.ru/ru/uroki/vse-uroki-spiskom/157-urok-92-service-prostoj-primer.html)

Пример запуска:

```java
Intent intent = new Intent(this, SomeService.class);
intent.setData(someData);
startService(intent);

или
Intent intent = new Intent();
intent.setAction(Intent.SOME_ACTION);
intent.putExtra(Intent.EXTRA_TEXT, "some message");
startService(intent);
```

### Запуск со стороны Android

```text
Все запущенные сервисы и попытки подключения к ним:
$ adb shell dumpsys activity services

Запустить сервис через дрозер:
$ run app.service.start --action <action> --component <package> <service>
вместо <action> можно использовать <service>

Сообщения (--msg) - это действительно 3 цифры хз для чего
```

