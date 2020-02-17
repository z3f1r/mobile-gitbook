# Activities

### Intro

Могут быть вызваны любыми приложениям, если установлены флаги exported и enabled. Это может позволить загружать формы в обход блокировок \(если не правильно настроено что или не делаются доп проверки\). По умолчанию, exported=false, но если объявлен intent-filter, то true.  
Активити могут гарантировать правильное поведение, если отслеживают состояние приложения. Например, если приложение находится в заблокированном состоянии, перепрыгивать на LockScreen.

### Жизненный цикл активити

![](../../../../.gitbook/assets/izobrazhenie%20%285%29.png)

### Пример объявления активити

![](../../../../.gitbook/assets/izobrazhenie%20%288%29.png)

### Пример запуска Activity

```java
public static Intent getStartingIntent(Context context, String userId) {
 Intent i = new Intent(context, UserDetailsActivity.class);
 i.putExtra(EXTRA_USER_ID, userId);
 return i;
}
```

Здесь мы видим, что форма принимает userId - строку. Это хороший подход. Если бы принимался объект, то это плохо \(чем?\)

Хорошая практика избегать использования intent-filters для приватных активити

На стороне Activity

```java
.. onCreate(Bundle bundle) {
 Intent intent = getIntent()
 intent.getString(...)
}
```

Общий вид вызова Intent для Activity: 

```java
Intent(MainActivity.this, TargetActivity.class)
```

