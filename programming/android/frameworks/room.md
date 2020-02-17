# Room

`Room` - обертка над `SQLIte/Realm/DAO`, представленная на `Goolge I/O`. Room provides an abstraction layer over SQLite in a similar way to Retrofit with network requests.

### Пример использования

`androidx.room.`  
[https://habr.com/ru/post/336196/](https://habr.com/ru/post/336196/)  
[https://developer.android.com/training/data-storage/room/accessing-data](https://developer.android.com/training/data-storage/room/accessing-data)

### Безопасность

Выглядит как достаточно безопасная штука, которая переводит объекты сразу в базу и обратно. Если делать все по инструкции, ошибиться невозможно \(про sqli\)  
Подробно: [https://medium.com/@appmattus/android-security-sql-injection-with-the-room-persistence-library-69f4e286960f](https://medium.com/@appmattus/android-security-sql-injection-with-the-room-persistence-library-69f4e286960f)

### С точки зрения разработки

Три основных компонента - `Entity`, `Dao`, `Database`  
`Entity` - объект, который хотим хранить в базе  
`Database` -  
`Dao` -

