# Intents

## Intro

### Составные части

* Component
* Action
* Extra data

### Для чего используются

Используются в IPC для:

* Запуска Activity \(обычно для запуска UI\)
* Broadcast информирование системы и других приложений
* Запуск/останов и взаимодействие с сервисами, запущенными в background'е
* Доступ к данным через ContentProvider
* как callback для событий

### Public and private component

Компоненты, доступные через Intents, могут быть public или private. Значение по умолчанию зависит от intent-filter, и легко, ошибочно, позволить компоненту быть или стать public. Чтобы избежать этого, в манифесте приложения можно установить компонент как android: `exported = false`.

Public компоненты, объявленные в манифесте, по умолчанию открыты, поэтому любое приложение может получить к ним доступ. Если к компоненту нет необходимости обращаться всем другим приложениям, рассмотрите возможность установки разрешения для компонента, объявленного в манифесте.

`Данные, полученные общедоступными компонентами, не могут быть доверенными и должны быть тщательно изучены.`

### Inter-app and Intra-app communications

Inter-app - между приложениями  
Intra-app - ежду компонентами одного приложения

### Explicit and Implicit

Explicit - отправка конкретному компоненту  
Implicit - отправка по всем компонентам, intent-filter которых зарегистрирован в манифесте \(и соотв в системе\). В этом случае пользователю будет предоставлен выбор, каким приложением обработать интент.  
Отдельно от них стоит Broadcast Intent - этот тип интентов дойдет всем таргетам, а не выбранному.

## Security Mistakes

Не передавать ценную информацию через BroadcastIntent между приложениями. Использовать Explicit Intents



## Примеры

### Запуск другого приложения

```kotlin
val intent_remote = Intent("${TARGET}.action.AUTH_REGISTRATION").apply {
    component = ComponentName(TARGET, "com.zfr.ctfzoneclient.service.view.AuthService")
    putExtra("USER_NAME", "testuser")
    putExtra("FIRST_NAME", "firstname_testuser")
    putExtra("LAST_NAME", "lastname_testuser")
    putExtra("PENDING_INTENT", pendingIntent)
}

```

