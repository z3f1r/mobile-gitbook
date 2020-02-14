# Basic

Дефолтный способ получение доступа к `view` объектам - использование `findViewbyId()`, но это неудобно, когда надо обновлять данные постоянно. 

### Плюсы использования data binding

* Меньше кода, легче читается
* `data` и `view` полностью разделены
* `data binding` строится только в при старте приложения, а не в рантайме \(как с `findViewbyId`\)
* Получаем `type safety` в коробке

### Подключаем data binding в проект

Открываем `build.gradle (Module: app)`. В секцию `android` добавляем:

```text
dataBinding {
    enabled = true
}
```

### Пример использования

[https://codelabs.developers.google.com/codelabs/kotlin-android-training-data-binding-basics/index.html](https://codelabs.developers.google.com/codelabs/kotlin-android-training-data-binding-basics/index.html)

