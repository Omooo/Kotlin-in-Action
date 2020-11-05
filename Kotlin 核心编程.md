---
Kotlin 核心编程
---

#### 第一章: 认识 Kotlin

1.

在 Kotlin 中，可以使用单行表达式来定义函数的语法，而不需要使用 return 来返回结果值。

```kotlin
// 初始函数
fun foo(x: Int): Int {
    return x + 1
}
// 使用单行表达式作为返回值，即表达式函数体
fun foo(x: Int): Int = x + 1
// 省略返回值类型
fun foo(x: Int) = x + 1
```

2.

智能类型推导。

```kotlin
// Java 中的写法
if (parentView instanceof ViewGroup) {
    ((ViewGroup) parentView).addView(childView);
}
// Kotlin 写法
if (parentView is ViewGroup) {
    parentView.addView(childView);
}
```

#### 第二章: 基础语法

1.

什么时候需要显式声明类型？

如果它是一个函数的参数？必须使用。

如果它是一个非表达式定义的函数？除了返回 Unit，其他情况必须使用。

如果它是一个递归的函数？必须使用。

如果它是一个公有方法的返回值？为了更好的代码可读性及输出类型的可控性，建议使用。

除上述情况之外，你可以尽量尝试不显式声明类型。

2.

在 Kotlin 编程中，我们推荐优先使用 val 来声明一个不可变的变量，这是一种防御性的编程思维模式，更加安全可靠。

3.

Kotlin 天然支持了部分函数式特性。函数式语言一个典型的特征就在于函数是头等公民 --- 我们不仅可以像类一样在顶层直接定义一个函数，也可以在一个函数内部定义一个局部函数。类似以下：

```kotlin
fun foo(x: Int): Int {
    fun double(x: Int): Int {
        return x * x
    }
    return double(x)
}
```

此外，我们还可以直接将函数像普通变量一样传递给另外一个函数，或在其他函数内被返回。这个就是所谓的**高阶函数**，你可以把它理解成 “以其他函数作为参数或返回值的函数”。

4.

Lambda 表达式本质上是一种语法糖，可以把它理解为简化表达后的匿名函数。

示例：

```kotlin
val sum = { i1: Int, i2: Int -> i1 * i2 }
```

在看这样一个例子：

```kotlin
listOf(1, 2, 3).forEach { println(it) }
```

这里的 it 是 Kotlin 简化 Lambda 表达的一种语法糖，叫作单个参数的隐式名称。

5.

函数、Lambda 和闭包的区分？

在你不熟悉 Kotlin 语法的情况下，很容易对 fun 声明的函数、Lambda 表达式的语法产生混淆，因为它们都可以存在花括号。现在我们已经了解了它们具体的语法，可通过以下的总结来更好的区分。

fun 在没有等号、只有花括号的情况下，是我们最常见的代码块函数体，如果返回非 Unit 值，必须带 return。示例：

```kotlin
// 返回 Unit 类型
fun print(i: Int) {
    println(i)
}
// 返回 Int 类型
fun print(i: Int): Int {
    return i * i
}
```

fun 带有等号，是单表达式函数体，这种情况下可以省略 return，示例：

```kotlin
fun print(i: Int): Int = i * i
```

不管是用 val 还是 fun，如果是等号加花括号的语法，那么构建的就是一个 Lambda 表达式，Lambda 的参数在花括号内部声明。所以，如果左侧是 fun，那么就是 Lambda 表达式函数体，也必须通过 () 或 invoke 来调用 Lambda，如：

```kotlin
// sum.invoke(2, 2) 或 sum(1, 2) 来调用
val sum = { i1: Int, i2: Int -> i1 * i2 }
// sum(1).invoke(2) 或 sum(1)(2) 来调用
fun sum(x: Int) = { y: Int -> x * y }
```

在 Kotlin 中，你会发现匿名函数体、Lambda（以及局部函数、object 表达式）

