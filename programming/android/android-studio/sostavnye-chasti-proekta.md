# Составные части проекта

## Пример проекта

![](../../../.gitbook/assets/izobrazhenie%20%2826%29.png)

## settings.gradle

```text
include ':app', ':core'
rootProject.name = "TestKotlinGame"
```

В этом файле перечисляются модули \(в gradle описывается как project\). Что бы переименовать модуль, достаточно переимееновать директорию и поменять подключение в этом файле

## local.properties

Путь до SDK

