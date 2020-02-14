# index

Room - обертка над SQLIte/Realm/DAO, представленная на Goolge I/O. Room provides an abstraction layer over SQLite in a similar way to Retrofit with network requests. Пример использования: androidx.room. [https://habr.com/ru/post/336196/](https://habr.com/ru/post/336196/) [https://developer.android.com/training/data-storage/room/accessing-data](https://developer.android.com/training/data-storage/room/accessing-data)

Выглядит как достаточно безопасная штука, которая переводит объекты сразу в базу и обратно. Если делать все по инструкции, ошибиться невозможно \(про sqli\) Подробно: [https://medium.com/@appmattus/android-security-sql-injection-with-the-room-persistence-library-69f4e286960f](https://medium.com/@appmattus/android-security-sql-injection-with-the-room-persistence-library-69f4e286960f)

## С точки зрения разработки

Поэтапное создание [https://codelabs.developers.google.com/codelabs/android-room-with-a-view-kotlin/\#4](https://codelabs.developers.google.com/codelabs/android-room-with-a-view-kotlin/#4)

### Подключение в проект

В build.gradle файл проекта добавьте репозитарий google\(\)

```text
allprojects {
    repositories {
        jcenter()
        google()
    }
    ...
}
```

В build.gradle файле модуля добавьте dependencies:

```text
dependencies {
    implementation "android.arch.persistence.room:runtime:1.0.0"
    annotationProcessor "android.arch.persistence.room:compiler:1.0.0"
    ...
}
```

### Структура

Три основных компонента - `Entity`, `Dao`, `Database` `Entity` - объект, который хотим хранить в базе. Используется для создания таблицы. `Database` - `Dao` - описывает методы для работы с базой данных.

#### Entity

```java
@Entity
public class Employee {

   @PrimaryKey
   public long id;

   public String name;

   public int salary;
}
```

По умолчанию, имя создаваемой таблицы будет название класса. Следующим образом задается свое название таблицы:

```java
@Entity(tableName = "employees")
public class Employee {
   // ...
}
```

Переопределить имя поля:

```java
@Entity()
public class Employee {
   @PrimaryKey()
   public long id;

   @ColumnInfo(name = "full_name")
   public String fullName;
   public int salary;
}
```

По умолчанию `Room` определяет тип данных для поля в таблице по типу данных поля в `Entity` классе. Но мы можем явно указать свой тип.

```java
@Entity()
public class Employee {

   @PrimaryKey(autoGenerate = true)
   public long id;
   public String name;

   @ColumnInfo(typeAffinity = TEXT)
   public int salary;
}
```

У `PrimaryKey` есть параметр `autoGenerate`. Он позволяет включить для поля режим `autoincrement`, в котором база данных сама будет генерировать значение, если вы его не укажете.

Чтобы создать составной ключ, используйте параметр primaryKeys.

```java
@Entity(primaryKeys = {"key1", "key2"})
public class Item {
   public long key1;
   public long key2;
   // ...
}
```

Внешний ключ \(`ForeignKeys`\) Внешние ключи позволяют связывать таблицы между собой. Если вы еще не знакомы с ними, то можете почитать о них в инете. В выше рассмотренных примерах у нас есть класс `Employee` для хранения данных по сотрудникам. Давайте создадим класс `Car` для хранения данных по машинам. И каждая машина должна быть прикреплена к какому-либо сотруднику.

```java
@Entity(foreignKeys = @ForeignKey(entity = Employee.class, parentColumns = "id", childColumns = "employee_id"))
public class Car {

   @PrimaryKey(autoGenerate = true)
   public long id;
   public String model;
   public int year;

   @ColumnInfo(name = "employee_id")
   public long employeeId;
}
```

Индекс \(Index\) Индексы могут повысить производительность вашей таблицы. Если вы еще не знакомы с ними, то можете почитать о них в инете. В аннотации `Entity` есть параметр `indicies`, который позволяет задавать индексы.

```java
@Entity(indices = {
               @Index("salary"),
               @Index(value = {"first_name", "last_name"})
           }
       )
public class Employee {
   @PrimaryKey(autoGenerate = true)
   public long id;

   @ColumnInfo(name = "first_name")
   public String firstName;

   @ColumnInfo(name = "last_name")
   public String lastName;
   public int salary;
}
```

Создаем два индекса: один по полю `salary`, а другой по двум полям `first_name` и `last_name`.

Индекс для одного поля также может быть настроен через параметр `index` аннотации `ColumnInfo`

```java
@Entity()
public class Employee {

   @PrimaryKey(autoGenerate = true)
   public long id;
   public String name;

   @ColumnInfo(index = true)
   public int salary;
}
```

Будет создан индекс для поле `salary`.

Вложенные объекты Пусть у нас есть класс `Address`, с данными о адресе. Это обычный класс, не `Entity`.

```java
public class Address {
   public String city;
   public String street;
   public int number;
}
```

И мы хотим использовать его в Entity классе Employee

```java
@Entity()
public class Employee {
   @PrimaryKey(autoGenerate = true)
   public long id;
   public String name;
   public int salary;
   @Embedded
   public Address address;
}
```

Простое решение - использовать аннотацию Embedded. Если у вас получается так, что совпадают имена каких-то полей в основном объекте и в Embedded объекте, то используйте префикс для Embedded объекта.

```java
@Embedded(prefix = "address")
public Address address;
```

В этом случае к именам полей Embedded объекта в таблице будет добавлен указанный префикс.

Ignore Аннотация Ignore позволяет подсказать Room, что это поле не должно записываться в базу или читаться из нее.

```java
@Entity
public class Employee {
   @PrimaryKey
   public long id;

   public String name;
   public int salary;

   @Ignore
   public Bitmap avatar;
}
```

#### Dao

Обратите внимание, что в качестве имени таблицы мы используем `employee`. Напомню, что имя таблицы равно имени `Entity` класса, т.е. `Employee`, но в `SQLite` не важен регистр в именах таблиц, поэтому можем писать `employee`. Для вставки/обновления/удаления используются методы `insert/update/delete` с соответствующими аннотациями. Тут никакие запросы указывать не нужно. Названия методов могут быть любыми. Главное - аннотации.

```java
@Dao
public interface EmployeeDao {

   @Query("SELECT * FROM employee")
   List<Employee> getAll();

   @Query("SELECT * FROM employee WHERE id = :id")
   Employee getById(long id);

   @Insert
   void insert(Employee employee);

   @Update
   void update(Employee employee);

   @Delete
   void delete(Employee employee);

}
```

#### Database

Аннотацией `Database` помечаем основной класс по работе с базой данных. Этот класс должен быть абстрактным и наследовать `RoomDatabase`.

```java
@Database(entities = {Employee.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
   public abstract EmployeeDao employeeDao();
}
```

В параметрах аннотации `Database` указываем, какие `Entity` будут использоваться, и версию базы. Для каждого `Entity` класса из списка `entities` будет создана таблица.

В `Database` классе необходимо описать абстрактные методы для получения `Dao` объектов, которые вам понадобятся.

### Как пользоваться

`Database` объект - это стартовая точка. Его создание выглядит так:

```java
AppDatabase db =  Room.databaseBuilder(getApplicationContext(),
       AppDatabase.class, "database").build();
```

Используем `Application Context`, а также указываем `AppDatabase` класс и имя файла для базы.

Учитывайте, что при вызове этого кода `Room` **каждый раз** будет создавать новый экземпляр `AppDatabase`. Эти экземпляры очень тяжелые и рекомендуется использовать один экземпляр для всех ваших операций. Поэтому вам необходимо позаботиться о синглтоне для этого объекта. Это можно сделать с помощью `Dagger`, например.

Если вы не используете `Dagger` \(или другой DI механизм\), то можно использовать `Application` класс для создания и хранения `AppDatabase`:

```java
public class App extends Application {

    public static App instance;

    private AppDatabase database;

    @Override
    public void onCreate() {
        super.onCreate();
        instance = this;
        database = Room.databaseBuilder(this, AppDatabase.class, "database")
                .build();
    }

    public static App getInstance() {
        return instance;
    }

    public AppDatabase getDatabase() {
        return database;
    }
}
```

Не забудьте добавить `App` класс в манифест

В коде получение базы будет выглядеть так:

```java
AppDatabase db = App.getInstance().getDatabase();
```

Из Database объекта получаем Dao.

```java
EmployeeDao employeeDao = db.employeeDao();
```

Теперь мы можем работать с `Employee` объектами. Но эти операции должны выполняться не в `UI` потоке. Иначе мы получим `Exception`.

Добавление нового сотрудника в базу будет выглядеть так:

```java
Employee employee = new Employee();
employee.id = 1;
employee.name = "John Smith";
employee.salary = 10000;

employeeDao.insert(employee);
```

Метод getAll вернет нам всех сотрудников в List

```java
List<Employee> employees = employeeDao.getAll();
```

Получение сотрудника по id:

```java
Employee employee = employeeDao.getById(1);
```

### UI поток

Операции по работе с базой данных - **синхронные**, и должны выполняться не в `UI` потоке.

В случае с `Query` операциями мы можем сделать их асинхронными используя `LiveData` или `RxJava`.

