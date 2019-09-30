---
Kotlin in Action
---

### 前言

多说无益，上代码～

### 示例

##### 替代 foreach、fori 循环

通常遍历 List 的时候，我们喜欢用 foreach 循环，但是 foreach 拿不到 index，这时候可能被迫用 fori 循环，fori 循环里面还要通过 get 拿 value，看看 Kotlin 如何做的：

```kotlin
val list = listOf("23", "2333", "23333")
for (item in list.withIndex()) {
    println("index:${item.index} value:${item.value}")
}
```

类似遍历 Map 的思想～

##### 嵌套循环时优雅的跳出最外层循环

跳出循环往往用 bread、continue，但是这些关键字只能作用于当前循环，想跳出外层循环，我们往往是加一个 boolean 类型的状态值，看看 Kotlin 如何做的：

```kotlin
loop@ for (j in 1..100) {
    for (i in 1..10) {
        if (i + j == 100) {
            println(i)
            println(j)
            break@loop
        }
    }
}
```

