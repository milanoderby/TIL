# Kotlin docs - Idiom

### Lazy property﻿

```kotlin
val p: String by lazy {
    // compute the string
}
```

<br>

### try-catch expression﻿

```kotlin
fun test() {
    val result = try {
        count()
    } catch (e: ArithmeticException) {
        throw IllegalStateException(e)
    }

    // Working with result
}
```

<br>

### Builder-style usage of methods that return Unit﻿

```kotlin
fun arrayOfMinusOnes(size: Int): IntArray {
    return IntArray(size).apply { fill(-1) }
}
```

참고자료: https://stackoverflow.com/questions/47955058/the-builder-style-usage-of-methods-that-return-unit-on-kotlin-website-confuse

<br>

### Java 7's try-with-resources﻿

```kotlin
val stream = Files.newInputStream(Paths.get("/some/file.txt"))
stream.buffered().reader().use { reader ->
    println(reader.readText())
}
```

<br>

### Swap two variables﻿

```kotlin
var a = 1
var b = 2
a = b.also { b = a }
```

참고자료: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/also.html