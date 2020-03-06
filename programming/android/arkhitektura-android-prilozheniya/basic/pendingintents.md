# PendingIntents

### Intro

[https://startandroid.ru/ru/uroki/vse-uroki-spiskom/160-urok-95-service-obratnaja-svjaz-s-pomoschju-pendingintent.html](https://startandroid.ru/ru/uroki/vse-uroki-spiskom/160-urok-95-service-obratnaja-svjaz-s-pomoschju-pendingintent.html)

Инструмент для взаимодействия между компонентами приложений.

Позволяет передать Intent другому приложению, которое сможет его выполнить, как будто его выполняет первое приложение \(т.е. с теми же permissions\). Это позволяет другим приложения возвращать информацию private компонентам родительского приложения.  
Внешнее приложение, если оно вредоносное, может попытаться повлиять на родительское приложение и/или данные/целостность

### Пример общего подхода

```kotlin
val intent = Intent(context, LocalService::class.java) // создаем интент для работы с локальным объектом
val pendingIntent = PendingIntent.getService(context, SOME_CODE, intent, SOME_FLAG) // помещаем локальный интент в pendingintent
val intent = Intent("com.other.app.action")
intent.putExtra("TestPI", pendingIntent)

На стороне принимающей стороны:
val intent = intent?.getParcelableExtra<PendingIntent>("TestPI")
```

### 

### Как хорошо делать

Использовать PendingIntent как отложенные функции возврата для private BroadcastReceivers или broadcast activities, и указывать полное имя компонента в базовом Intent

### Пример SafePendingIntent

```java
//explicit (to MyService)
Intent intent = new Intent(context, MyService.class);
PendingIntent pi = PendingIntent.getService(getApplicationContext(), 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);

//implicit
Intent intent = new Intent("com.my.app.action")
PendingIntent pi = PendingIntent.getService(getApplicationContext(), 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);
```

Разобранные примеры уязвимостей \(прочесть!!\)

{% file src="../../../../.gitbook/assets/esorics18.pdf" %}

