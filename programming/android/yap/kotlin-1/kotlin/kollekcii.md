# Коллекции



```kotlin
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
```

