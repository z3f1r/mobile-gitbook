# bundletool \(aab -&gt; apk\)

Есть такой формат дистрибьюции Andoid-приложений как aab. Он применяется для отправки приложения в Google Play Market. Чтобы его перегнать в apk, нужен инструмент bundletool.

Пример использования:

```text
java -jar bundletool-all-1.5.0.jar build-apks --bundle=app.aab --mode=universal --output=test.apks
или если ставили через homebrew, то просто bundletool ...
```

Внутри ищем universal.apk -&gt; это она

