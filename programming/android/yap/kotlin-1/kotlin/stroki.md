# Строки

## Шаблоны

```kotlin
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
}
```

## Дополнить строку символами

```kotlin
val name = "Barsik"
val pad = name.padStart(10, '#')
println(pad) // ####Barsik

val name = "Barsik"
val pad = name.padEnd(10, '*')
println(pad) // Barsik****
```

## Подстроки

```kotlin
// подстрока с указанного индекса
val result = "developer.alexanderklimov.ru".substring(10) // alexanderklimov.ru

// подстрока до первого указанного разделителя
val first = "developer.alexanderklimov.ru".substringBefore('.') // developer

// подстрока после первого указанного разделителя
val last = "developer.alexanderklimov.ru".substringAfter('.') // alexanderklimov.ru

// подстрока после последнего указанного разделителя
val last = "developer.alexanderklimov.ru".substringAfterLast('.') // ru

// подстрока до последнего указанного разделителя
val beforeLast = "developer.alexanderklimov.ru".substringBeforeLast('.') // developer.alexanderklimov
```

## Встроенные функции

```kotlin
val blank = "   ".isBlank() // true, если пустая строка или пустые символы пробела, табуляции и т.п.

// индекс последнего символа
val lastIndex = "Кот Мурзик".lastIndex // 9

// переводим в верхний регистр первый символ строки
// decapitalize() выполняем обратную задачу
val capitalize = "кот Мурзик".capitalize()

val withSpaces = "1".padStart(2) // добавляем пробел перед строкой
val endZeros = "1".padEnd(3, '0') // "100"  добавляем нули в конец

val dropStart = "Kotlin".drop(2) // "tlin" убираем первые символы в указанном количестве
val dropEnd = "Kotlin".dropLast(3) // "Kot" убираем последние символы в указанном количестве

// возвращаем строку без первого символа, который удовлетворяет условию
val string = "Мурзик"
val result = string.dropWhile{
    it == 'М'
}
textView.text = result // урзик

// возвращаем строку без последнего символа, который удовлетворяет условию
val string = "Мурзик"
val result = string.dropLastWhile{
    it == 'к'
}
textView.text = result // Мурзи

// разбиваем на массив строк
"A\nB\nC".lines() // [A, B, C]
"ABCD".zipWithNext() // [(A, B), (B, C), (C, D)]

// удаляем символы из заданного диапазона
val string = "Кот, который гулял сам по себе"
val result = string.removeRange(
    3..28 // range
)

// удаляем префикс из строки
val string = "Кот, который гулял сам по себе"
val result = string.removePrefix("Кот")

// удаляем суффикс из строки
val string = "Кот, который гулял сам по себе"
val result = string.removeSuffix("себе")

// удаляем заданный разделитель, который должен окружать строку с начала и с конца
val string = "та, тра-та-та, мы везём с собой кота"

val result = string.removeSurrounding(
    "та" // delimiter
)

textView.text = result // , тра-та-та, мы везём с собой ко

// Также можно указать разные начало и конец, которые окружают строку
val string = "Тра-та-та, тра-та-та, мы везём с собой кота"

val result = string.removeSurrounding(
    "Тра-", // prefix
    " кота" // suffix
)

textView.text = result // та-та, тра-та-та, мы везём с собой


// Добавляем отступы при явном переводе на новую строку
val string = "Какой-то длинный текст, \nсостоящий из имён котов: " +
        "\nВаська" +
        "\nБарсик" +
        "\nРыжик"

val result = string.prependIndent(
    "     " // indent
)

// Разбиваем символы на две группы. 
// В первую группу попадут символы в верхнем регистре, во вторую - символы в нижнем регистре
val string = "Кот Васька и кот Мурзик - Друзья!"

val result: Pair<String, String> = string.partition {
    it.isUpperCase()
}

textView.text = result.first + ":" + result.second //КВМД:от аська и кот урзик - рузья!

// Разбиваем строку на список строк. В качестве разделителя - перевод на новую строку
val string = "Кот Васька\nКотМурзик\nКот Мурзик"

// Split string into lines (CRLF, LF or CR)
val lines: List<String> = string.lines()

textView.text = "Кол-во строк: ${lines.size}"
lines.forEach {
    textView.append("\n" + it)
}

// Разбиваем строку на список строк с указанным числом символов. В последней строке может выводиться остаток
val string = "Тра-та-та, тра-та-та, мы везём с собой кота"
val list:List<String> = string.chunked(11)
list.forEach {
    textView.append("$it\n")
}

/*
Тра-та-та, 
тра-та-та, 
мы везём с 
собой кота
*/

// Содержит ли строка только цифры (используем предикат)
val string = "09032020"

// Returns true if all characters match the given predicate.
val result: Boolean = string.all{
    it.isDigit()
}
textView.append("Is the string $string contain only digit? $result")

// Содержит ли строка хотя бы одну цифру (используем предикат)
val string = "3 кота"
// Returns true if at least one character matches the given predicate.
val result: Boolean = string.any() {
    it.isDigit()
}
textView.append("Is the text \"$string\" contain any digit? $result")

// Сравниваем две строки с учётом регистра
val string1 = "This is a sample string."
val string2 = "This is a SAMPLE string."

if (string1.compareTo(string2, ignoreCase = true) == 0) {
    textView.append("\n\nBoth strings are equal, ignoring case.")
} else {
    textView.append("\n\nBoth strings are not equal, ignoring case.")
}
```

Можно фильтровать данные с помощью **filter\(\)**. Допустим, мы хотим извлечь только цифры из строки.

```text

val string = "9 жизней (2016) - Nine Lives - информация о фильме"
val filteredText = string.filter { it.isDigit() }
textView.text = filteredText // 92016
```

Если хочется решить обратную задачу и получить только символы, но не цифры - то достаточно вызвать **filterNot\(\)**.

```text

val filteredText = string.filterNot { it.isDigit() }
```

