---
description: Node.js Framework
---

# React Native

## Install

link: [https://reactnative.dev/docs/0.61/getting-started](https://reactnative.dev/docs/0.61/getting-started)

## Пакетный менеджер

yarn или npx

## Usage

Create App:

```text
npx react-native init MyTestApp [--template <Template Name>] 
```

Или установить из репозитория

Далее, надо подтянуть все зависимости: `npm install` или `yarn install`

Теперь можно запускать: 

```text
npx react-native start (запускаем упаковщик)
npx react-native run-ios (или run-android; собираем проект, подключаемся упаковщику за новым бандлом)

npx react-native run-android  --deviceId 64fe7667d340
```

## Меняем код

Открываем App.js, изменяем. 

Нажимаем R дважды или выбираем Reload из developer Menu \(cmd-M\) для отображения наших изменений.

Tutorial - для дальнейшего погружения: [https://reactnative.dev/docs/0.61/tutorial](https://reactnative.dev/docs/0.61/tutorial)

Add some code in to existing app: [https://reactnative.dev/docs/0.61/integration-with-existing-apps](https://reactnative.dev/docs/0.61/integration-with-existing-apps) 

## Debug

Может потребоваться прокинуть порт: `adb reverse tcp:8081 tcp:8081`

Можно с помощью Chrome DevTools/Safari/..

Можно с помощью React DevTools

```text
npm install -g react-devtools
react-devtools
```

При дебаге ReactNative приложения в Metro Server запускается не вшитый в apk js-код \(его там вообще нет\), а из проекта напрямую, как есть.

Следовательно, все console.log будут выводиться   
Смотреть их можно не только adb logcat --pid=&lt;pid&gt;,   
но и через npx react-native log-android

### Логи

```text
$ npx react-native log-ios
$ npx react-native log-android
```

### Debugger Integration

Еще статья про debugger integration: [https://github.com/jhen0409/react-native-debugger/blob/master/docs/debugger-integration.md](https://github.com/jhen0409/react-native-debugger/blob/master/docs/debugger-integration.md)

