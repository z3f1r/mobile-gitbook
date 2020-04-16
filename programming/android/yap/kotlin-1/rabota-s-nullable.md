# Работа с nullable

```kotlin
1. Elvis operator "?:" - вернет значение или сделает действие (return)
test = value ?: return

2. Use safee call - выполнится если не null
left?.let { node -> queue.add(node) }
left?.let { queue.add(it) }
left?.let(queue::add)

3. 
```

