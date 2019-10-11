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

详见：[标签处返回](https://www.kotlincn.net/docs/reference/returns.html#%E6%A0%87%E7%AD%BE%E5%A4%84%E8%BF%94%E5%9B%9E))



### Kotlin/Everywhere Beijing 2019

[用 Kotlin 提升你的开发效率 --- 陈龙博](http://www.itdks.com/Course/detail?id=117253)

##### 中缀表达式

```kotlin
object Main {

    @JvmStatic
    fun main(args: Array<String>) {
        val xiaoM = User("xiaoM", 18)
        val xiaoH = User("xiaoH", 19)
        if (xiaoM sameAge xiaoH) {
            println("Same Age")
        }
    }

}

data class User(val name: String, val age: Int) {
    infix fun sameAge(user: User): Boolean {
        return this.age == user.age
    }
}
```

##### 标准函数

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val json = """
            {
                "name": "JSON"
            }
        """.trimIndent()
        dealCityInfo(JSONObject(json)) {
            Toast.makeText(this, "Fail", Toast.LENGTH_SHORT).show()
        }
    }

    private fun dealCityInfo(data: JSONObject?, fail: () -> Unit) {
        data?.takeIf {
            it.has("city_info")
        }?.takeIf {
            with(it.getJSONObject("city_info")) {
                return@takeIf has("title") && has("data")
            }
        }?.let {
            it.getJSONObject("city_info")
        }.apply {

        } ?: fail()
    }
```

##### 扩展函数

```kotlin
    fun main(context: Context) {
        println(1f.dp(context))
    }

    fun Float.dp(context: Context): Int {
        return TypedValue.applyDimension(
            TypedValue.COMPLEX_UNIT_DIP,
            this,
            context.resources.displayMetrics
        ).toInt()
    }
```

##### “真”泛型

```kotlin
    inline fun <reified T : Activity> Activity?.startActivity() {
        this?.startActivity(Intent(this, T::class.java))
    }
    
    // use
    startActivity<MainActivity>()
```

##### 集合操作

```kotlin
val list = listOf(0, 2, 5, -1)

var result = list.any {
    it > 1
}
println(result)
result = list.all {
    it > 1
}
println(result)
```

### Kotlin in Action

##### 让你的代码更简洁：局部函数和扩展

```kotlin
data class User(val name: String, val age: Int)

fun User.validateBeforeSave() {
    fun validate(boolean: Boolean) {
        if (!boolean) {
            throw IllegalArgumentException("")
        }
    }
    validate(this.name != "")
    validate(this.age > 18)
}

fun saveUser(user: User) {
    user.validateBeforeSave()
    // save data
    // ...
}
```

##### 对象表达式：改变写法的匿名内部类

object 关键字不仅仅能用来声明单例式的对象，还能用来声明匿名对象。匿名对象代替了 Java 中匿名内部类的用法。

```kotlin
var count = 0
tv_content.setOnClickListener(object : View.OnClickListener {
    override fun onClick(v: View?) {
        toast("Count: ${count++}")
    }
})
```

与 Java 匿名内部类只能扩展一个类或实现一个接口不同，Kotlin 的匿名对象可以实现多个接口或者不实现接口。

注意：与对象声明不同，匿名对象不是单例的。每次对象表达式被执行都会创建一个新的对象实例。

与 Java 的匿名类一样，在对象表达式中的代码可以访问创建它的函数中的变量。但是与 Java 不同，访问并没有被限制在 final 变量，还可以在对象表达式中修改变量的值。

对象表达式在需要在匿名对象中重写多个方法时是最有用的，如果只需要实现一个单方法的接口，用 Lambda 表达式即可。