---
description: Retrofit2 - Фреймворк позволяющий строить API (мб они генеряться?)
---

# Intro

### Документация

[https://square.github.io/retrofit/\#api-declaration](https://square.github.io/retrofit/#api-declaration)

### О классах

Пусть Class - это модель, которая описывает какой-то объект  
Тогда:  
1. Class.java  
Содержит все поля объекта. Их геттеры, а также метод сравнения объектов  
2. Class$Builder.java  
Содержит все сеттеры объекта + метод build\(\), который собирает все поля в объект Class

1. Class\_\*TypeAdapter.java \(Например, Class\_JsonTypeAdapter\) Содержит в себе методы read/write, которые переводят объект Class в контейнер \(н-р, Json\) и обратно.

Пример использования с courootines: [https://www.c-sharpcorner.com/article/how-to-use-retrofit-2-with-android-using-kotlin/](https://www.c-sharpcorner.com/article/how-to-use-retrofit-2-with-android-using-kotlin/)

