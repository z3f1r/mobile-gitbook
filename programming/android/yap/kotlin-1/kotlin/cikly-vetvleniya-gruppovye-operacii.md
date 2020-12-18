# Циклы, ветвления, групповые операции

```kotlin
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
    
// триарные операторы
fun max(a: Int, b: Int) = if (a > b) a else b

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
```

