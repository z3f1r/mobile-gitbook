# Static Analys

## Проверить, что приложение содержит расширения 

### XCode

cmd+shift+f и ищем `NSExtensionPointIdentifier`

или открываем Build Phases / Embed App exetensions

Все, что с расширением .appex - расширения, теперь можем перейти в сорцы

### IPA

Грепаем по bundl'у

```text
$ grep -nr NSExtensionPointIdentifier Payload/Telegram\ X.app/
```

или заходим на девайс плагины приложения на устройстве:

```text
/var/containers/Bundle/Application/15E6A58F-1CA7-44A4-A9E0-6CA85B65FA35/Telegram X.app/PlugIns
```

## Определить поддерживаемые типы данных

В Info.plist ищем строку `NSExtensionActivationRule`

## Проверить обмен данными с Containing App

## Проверить, ограничивает ли приложение использование расширений

