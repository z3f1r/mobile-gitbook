# Nullable

```kotlin
// Nullable-значения и проверка на null
fun var_null_and_check(str: Any): Int? {
    // Возвращает null если str не содержит числа
    // Ссылка должна быть явно объявлена как nullable (символ ?) когда она может принимать значение null.
    val x = null
    if (str is String || str == null)
        return 1
        //return str.length
    else {
        if (str !is String)
            return 1
        return 0
    }
}
```



```kotlin
// Сокращение для "Если не null"
val files = File("Test").listFiles()
println(files?.size)

// Сокращение для "Если не null, иначе"
println(files?.size ?: "empty")

// Вызов оператора при равенстве null
/*
val email = name["email"] ?: throw IllegalStateException("Email is missing!")
*/

// Выполнение при неравенстве
/*
val data = ...

data?.let {
    ... // execute this block if not null
}
*/

```

