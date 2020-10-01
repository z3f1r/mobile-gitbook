# ReactNative

## Install

link: [https://reactnative.dev/docs/0.61/getting-started](https://reactnative.dev/docs/0.61/getting-started)

## Usage

Create App:

```text
npx react-native init MyTestApp
```

Или установить из репозитория

Далее, надо подтянуть все зависимости: npm install или yarn install

Теперь можно запускать: 

```text
npx react-native start (запускаем упаковщик)
npx react-native run-ios (или run-android; собираем проект, подключаемся упаковщику за новым бандлом)

npx react-native run-android  --deviceId 64fe7667d340
```

## Debug

Может потребоваться прокинуть порт: `adb reverse tcp:8081 tcp:8081`

Можно с помощью Chrome DevTools/Safari/..

Можно с помощью React DevTools

```text
npm install -g react-devtools
react-devtools
```

Логи

```text
$ npx react-native log-ios
$ npx react-native log-android
```

