---
description: >-
  LiveData is an observable data holder class that is lifecycle-aware.
  Используется для обновления данных в UI автоматически.
---

# LiveData

### Характерные черты LiveData

LiveData is observablle  
LiveData сохраняет информацию. Является враппером для любого типа данных  
LiveData is lifecycle-aware. То есть, когда добавляем observer, observer ассоциируется с LifecycleOwner. LiveData будет обновляться, если lifecycle будет переходить в состояния STARTED или RESUMED.

Пример

```kotlin
class Test {
    val word = MutableLiveData<String>()
    
    init {
        word.value = ""
    }
    
    private fun nextWord() {
        word.value = "test_word"
    }
}

...
var test = Test()
test.word.observe(viewLifecycleOwner, Observer {newWord -> 
    binding.wordText.text = newWord
})
...
```

### LiveData и MutableLiveData

Разница: LiveData - readonly

Пример:

```kotlin
// The current word
private val _word = MutableLiveData<String>()
val word: LiveData<String>
   get() = _word
...
init {
   _word.value = ""
   ...
}
...
private fun nextWord() {
   if (!wordList.isEmpty()) {
       //Select and remove a word from the list
       _word.value = wordList.removeAt(0)
   }
}
```

### LiveData transformations

 При передаче из обного объекта в другой появляется необходимость в приминении преобразований. Для этого можно использовать Transformations

Пример

```kotlin
// The String version of the current time
val currentTimeString = Transformations.map(currentTime) { time ->
   DateUtils.formatElapsedTime(time)
}
```

