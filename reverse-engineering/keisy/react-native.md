---
description: >-
  При распаковке мобильного приложения, написанного на React Native, мы
  обнаружим
---

# React Native

При распаковке мобильного приложения, написанного на React Native, мы обнаружим в папке assets два файла: `index.android.bundle` и `index.android.bundle.map`

![](../../.gitbook/assets/izobrazhenie%20%284%29.png)

Это упакованный JS-код и смотреть его тяжело. Однако, можно его преобразовать следующим образом, если имеем файл `index.android.bundle.map` \(этот файл отвечает за маппинг исх кода\):  
создаем файл `index.html` со следующим содержимым: `<script src="index.android.bundle"></script>`.

Далее, открываем браузером \(Google, FF, etc\), лезем в сорцы и видим норм проект  


