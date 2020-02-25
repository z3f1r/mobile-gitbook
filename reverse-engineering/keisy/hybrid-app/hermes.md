# Hermes

Hermes - JS Engine. Перегоняет jS в байткод и от этого все работае быстрее

[https://facebook.github.io/react-native/docs/hermes](https://github.com/facebook/hermes)

О подключении в React Native проект [https://facebook.github.io/react-native/docs/hermes](https://facebook.github.io/react-native/docs/hermes)

Отличие от React Native в том, что index.android.bundle будет сериализован \(бинарщина\)

```text
$ hermes -b --dump-bytecode index.android.bundle
Error deserializing bytecode: Wrong bytecode version. Expected 74 but got 62%
$ hermes -version
74

Есть несколько релизов hermes:
v0.4.0 - 74
v0.3.* - 72
v0.2.1 - 62
```

