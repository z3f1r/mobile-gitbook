# Классы и функции

## Функции

```kotlin
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
}
```

## static

PARAM\_RESULT - статическое поле

```kotlin
class MainActivity : AppCompatActivity() {
    companion object {
        val PARAM_RESULT: String = "com.zfr.network.extra.RESULT"
        fun test() {
            
        }
    }    
}
```

