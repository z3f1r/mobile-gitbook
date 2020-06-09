# Push

APNs - Apple Push Notification service - системная штука для рассылки пушей: [https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html\#//apple\_ref/doc/uid/TP40008194-CH8-SW1](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1)

Под rich-нотификации подразумеваются кастомные виджеты \(UIViewController\), которые отрисовываются при выполнении force touch-действия по обычному системному уведомлению. Поддерживаются с iOS 10+.

Существует 2 типа extension по работе с push-уведомлениями: NotificationServiceExtension \([https://developer.apple.com/documentation/usernotifications/unnotificationserviceextension](https://developer.apple.com/documentation/usernotifications/unnotificationserviceextension)\) и NotificationContentExtension \([https://developer.apple.com/documentation/usernotificationsui/unnotificationcontentextension](https://developer.apple.com/documentation/usernotificationsui/unnotificationcontentextension)\). Оба живут в отдельных процессах – т.е. для общения с данными и ресурсами основного приложения необходимо выполнять специальные меры \(shared dara, keychain и т.д.\).

NotificationServiceExtension позволяет дополнительно обработать полученное системой push-уведомление до момента показа его в приложении \(например, часто используется для получения дополнительной информации - загрузка логотипа с собственного back-end\). Процесс запускается и функционирует в бэкграунде, но у него есть определённое отведённое время работы \(30 секунд\). Если за отведенное время контент уведомления не изменится и не вызовется соответствующий комплишен блок, то отобразится уведомление с тем содержимым, что пришло в коротком apns пуше.

NotificationContentExtension позволяет возвращать кастомный UIViewController для rich-нотификации. Данный контроллер должен инициализироваться с помощью storyboard и реализовывать протокол UNNotificationContentExtension для получения информации из пуш-уведомления.



