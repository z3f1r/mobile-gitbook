# PendingIntents

### Intro

[https://startandroid.ru/ru/uroki/vse-uroki-spiskom/160-urok-95-service-obratnaja-svjaz-s-pomoschju-pendingintent.html](https://startandroid.ru/ru/uroki/vse-uroki-spiskom/160-urok-95-service-obratnaja-svjaz-s-pomoschju-pendingintent.html)

Инструмент для взаимодействия между компонентами приложений.

Позволяет передать Intent другому приложению, которое сможет его выполнить, как будто его выполняет первое приложение \(т.е. с теми же permissions\). Это позволяет другим приложения возвращать информацию private компонентам родительского приложения.  
Внешнее приложение, если оно вредоносное, может попытаться повлиять на родительское приложение и/или данные/целостность

### Пример общего подхода

```kotlin
val intent = Intent(context, LocalService::class.java) // создаем интент для работы с локальным объектом - указываем, тем самым, кто будет обрабатывать ответ
val pendingIntent = PendingIntent.getService(context, SOME_CODE, intent, SOME_FLAG) // помещаем локальный интент в pendingintent
val intent = Intent("com.other.app.action") // Куда отправляем PendingIntent
intent.putExtra("TestPI", pendingIntent)

На стороне принимающей стороны:
val intent = intent?.getParcelableExtra<PendingIntent>("TestPI")
```

### Пример: из MainActivity отправляем интент в сервис и получаем ответ через PendingIntent обратно

```kotlin
В MainActivity:
val intent = Intent(this, MainActivity::class.java)
val pendingIntent = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT)
val intent_service = Intent(this, NetworkService::class.java).apply { формирую интент }
startService(intent_service)

Сервис запускается, обрабатывает что мне нужно. Настает время вернуть результат:
В NetworkService:
val output: String = «blabla»
pendingIntent.send(applicationContext, 0, Intent().putExtra(MainActivity.PARAM_RESULT, output))

И поток управления возвращается в MainActivity, в метод onCreate, а не в onActivityResult(requestCode: Int, resultCode: Int, data: Intent?), и, соответственно, активити перезагружается

Как мне вернуть результат в onActivityResult ?

UPD: с флагом intent.setFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP) активити не перезагружается, но результат в onActivityResult все равно не возвращается
UPD2: В MainActivity переопределяем onNewIntent -> сюда придет ответ PendingIntent с сервиса
```

### Как хорошо делать

Использовать PendingIntent как отложенные функции возврата для private BroadcastReceivers или broadcast activities, и указывать полное имя компонента в базовом Intent



### \(Эта штука не работает\) Передача пустых интентов в PendingIntent

В примере ниже в PendingIntent передается пустой интент. Если такой PendingIntent попадет к приложению атакующего, то он сможет выполнить любой код в пространстве уязвимого приложения \(через send отправляем свой интент к любому компоненту \(даже приватному\)\). Чтобы такого не было, надо в PendingIntent отправлять Intent с указанием action или компонента. Тогда интент придет именно компоненту-отправителю.

```kotlin
PendingIntent.getService(getApplicationContext(), 0, Intent(), 0)
```

### Intent Redirection

Уязвимость заключается в недостаточной валидации вложенных интентов

### Пример SafePendingIntent

```java
//explicit (to MyService)
Intent intent = new Intent(context, MyService.class);
PendingIntent pi = PendingIntent.getService(getApplicationContext(), 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);

//implicit
Intent intent = new Intent("com.my.app.action")
PendingIntent pi = PendingIntent.getService(getApplicationContext(), 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);
```

