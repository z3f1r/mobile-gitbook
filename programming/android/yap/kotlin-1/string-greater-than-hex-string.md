# String -&gt; Hex String

```kotlin
fun ByteArray.toHexString() = joinToString("") { "%02x".format(it) }

var someString = "test"
val result = someString.toByteArray().toHexString()
```

