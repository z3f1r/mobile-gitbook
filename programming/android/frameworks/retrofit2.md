---
description: Retrofit2 - Фреймворк позволяющий строить API (мб они генеряться?)
---

# Retrofit2

### SSL Pinning

SSLPinning \(делается через OkHTTP: сначала проверяем серт через okhttp, потом делаем запрос через ретрофит\)  
[https://proandroiddev.com/configuring-retrofit-2-client-in-android-130455eaccbd](https://proandroiddev.com/configuring-retrofit-2-client-in-android-130455eaccbd)

OkHTTP Client SSL Pinning ex:  
[https://www.codota.com/code/java/methods/okhttp3.OkHttpClient$Builder/sslSocketFactory](https://www.codota.com/code/java/methods/okhttp3.OkHttpClient$Builder/sslSocketFactory)

### О классах

Пусть Class - это модель, которая описывает какой-то объект  
Тогда:  
1. Class.java  
Содержит все поля объекта. Их геттеры, а также метод сравнения объектов  
2. Class$Builder.java  
Содержит все сеттеры объекта + метод build\(\), который собирает все поля в объект Class

1. Class\_\*TypeAdapter.java \(Например, Class\_JsonTypeAdapter\) Содержит в себе методы read/write, которые переводят объект Class в контейнер \(н-р, Json\) и обратно.

