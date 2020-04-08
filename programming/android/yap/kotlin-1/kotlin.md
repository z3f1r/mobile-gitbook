---
description: Основы синтаксиса
---

# Основы синтаксиса

Документация \(Language Guide\): [https://kotlinlang.org/docs/reference/exceptions.html](https://kotlinlang.org/docs/reference/exceptions.html)

```kotlin
package ctf.zone.pwn  // Имя пакета указывается в начале исходного файла, так же как и в Java
                      // Но в отличие от Java, нет необходимости, чтобы структура пакетов совпадала со структурой папок: исходные файлы могут располагаться в произвольном месте на диске.

// Полная документация: https://kotlinlang.ru/docs/reference/classes.html


import java.lang.IllegalStateException
import java.util.*
import java.io.File


val PI = 3.14
var global_var = 0

/* Блочный комментарий из
нескольких строк */

class KotlinClass {
    fun sum(a: Int, b: Int): Int {  // Функция принимает два аргумента Int и возвращает Int
        return a + b
    }

    fun sum(a: Int, b: Double) = a + b // Функция с выражением в качестве тела и автоматически определенным типом возвращаемого значения

    fun printSum(a: Int, b: Int): Unit {  // Функция, не возвращающая никакого значения (void в Java):
        print(a + b)
    }

    fun printSum(a: Int, b: Double) {  // Тип возвращаемого значения Unit может быть опущен
        print(a + b)
        println("jsfvkjsdfg")
    }

    fun variables() {
        // Неизменяемая (только для чтения) внутренняя переменная
        val a: Int = 1
        val b = 1   // Тип `Int` выведен автоматически
        val c: Int  // Тип обязателен, когда значение не инициализируется
        c = 1       // последующее присвоение


        // Изменяемая переменная
        var x = 5 // Тип `Int` выведен автоматически
        x += 1

        // Изменение глобальных переменных
        global_var += 1
    }

    fun string_patterns(args: Array<String>, test: Any) {
        /*
        Допустимо использование переменных внутри строк в формате $name или ${name}
         */
        if (args.size == 0) return
        else print("kjkfgd")

        print("Первый аргумент: ${args[0]}")

        var a = 1
        // просто имя переменной в шаблоне:
        val s1 = "a равно $a"

        a = 2
        // произвольное выражение в шаблоне:
        val s2 = "${s1.replace("равно", "было равно")}, но теперь равно $a"

        /*
          Результат работы программы:
          a было равно 1, но теперь равно 2
        */

        // Использование циклов
        for (arg in args)
            print(arg)

        for (i in args.indices)
            print(args[i])

        var i = 0
        while (i < args.size) {
            print(args[i++])
        }

        // Использование switch (здесь он when называется)
        when (test) {
            1 -> print("123")
            "Hello" -> print("jvcbsdjfv")
            is Long -> print("sjkdvnksdv")
            !is String -> print("sdfsdg")
            else -> print("Unknown")
        }

        // Использование интервалов
        if (i in 1..5)
            print("OK")
        if (i !in 1..5)
            print("FAIL")
        for (i in 1..5 step 1)
            print(i)
    }

    fun max(a: Int, b: Int) = if (a > b) a else b

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

    fun collections() {
        val items = setOf("apple", "banana", "kiwi")
        // Использование лямбда-выражения для фильтрации и модификации коллекции
        items
            .filter { it.startsWith("A") }
            .sortedBy { it }
            .map { it.toUpperCase() }
            .forEach { print(it) }

        // Read-only ассоциативный список (map)
        var map = mapOf("a" to 1, "b" to 2, "c" to 3)

        // Итерация по карте/списку пар
        for ((k, v) in map) {
            println("$k -> $v")
        }

        // Обращение к списку
        println(map["key"])

        // read-only list
        val list = listOf("a", "b", "c")
    }

    // Ленивые свойства
    val p: String by lazy {
        // compute the string
        return@lazy "sdfsdf"
    }

    fun fishechki() {
        // Функции-расширения
        fun String.spaceToCamelCase() { return }
        "Convert this to camelcase".spaceToCamelCase()

        // Создание синглтона
        /*
        object Resource {
            val name = "Name"
        }
        */

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

        // return с оператором when
        val color = "sfgsdg"
        var test = when (color) {
            "Red" -> 0
            "Green" -> 1
            "Blue" -> 2
            else -> throw IllegalArgumentException("Invalid color param value")
        }

        // try/catch
        val result = try {
            print("lkdfjvlkdfjvlkdfg")
        } catch (e: ArithmeticException) {
            throw IllegalStateException(e)
        }

        // Вызов нескольких объектов через with
        /*
        class Turtle {
            fun penDown()
            fun penUp()
            fun turn(degrees: Double)
            fun forward(pixels: Double)
        }

        val myTurtle = Turtle()
        with(myTurtle) { //draw a 100 pix square
            penDown()
            for(i in 1..4) {
                forward(100.0)
                turn(90.0)
            }
            penUp()
        }
        */
    }
}
```

