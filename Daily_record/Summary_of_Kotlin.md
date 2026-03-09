# Keep learning

# 1.变量

    // 常量（不可变）
    val constant: String = "Hello"
    
    // 变量（可变）
    var mutable: Int = 10
    
    // lateinit（类中延迟初始化）
    class Example {
        lateinit var initialized: String
    }

# 2. 逻辑控制

    // if
    if (age > 18) {
        println("Adult")
    } else if (age > 12) {
        println("Teen")
    } else {
        println("Child")
    }
    
    // when
    when (status) {
        "active" -> println("Active")
        "inactive" -> println("Inactive")
        else -> println("Unknown")
    }
    
    // while
    var i = 0
    while (i < 5) {
        println(i++)
    }
    
    // for-in
    for (item in listOf("A", "B", "C")) {
        println(item)
    }
    
    // 区间
    for (i in 1 until 5) { // 1 to 4
        print("$i ")
    }
    for (i in 5 downTo 1) { // 5,4,3,2,1
        print("$i ")
    }

# 3. 函数

## 一，普通函数

        基础语法：fun [函数名]([参数列表]): [返回类型] = [表达式] 或 { [代码块] }

### （1）带返回值的函数

           fun calculateSum(a: Int, b: Int): Int = a + b  // 表达式体

### （2）带代码块的函数（多行逻辑）

           fun isAdult(age: Int): Boolean {
           if (age >= 18) return true
           return false
        }
        
### （3）默认参数（避免重载）

        fun sendEmail(to: String, subject: String = "No Subject", body: String = "") {
            println("To: $to | Subject: $subject | Body: $body")
        }
        //调用示例：sendEmail("user@example.com", body = "Hello") 
    
## 二，标准函数
    
### （1）let: 

        它的意思就像“让这个东西去做点什么”。它非常适合用来安全地处理可能为空（null）的对象，
        或者对一个值进行临时的操作和转换。把它看作“安全操作符”。当一个对象可能为空时，你可以
        用 ?.let，意思是“只有当它不为空时，我才让它做点事情”。

          val length = text.let { 
             println("Inside let: $it") // it = "Kotlin"
             it.length // 返回长度
          } // length = 6
    
### （2）run: 

        作用域内使用this，返回结果

         val sum = text.run { 
            println("Inside run: $this") // this = "Kotlin"
            length + 10 // 返回 16
         } // sum = 16
    
### （3）apply: 

        作用域内使用this，**返回this**（链式调用）

            val sb = StringBuilder().apply {
                append("Hello") // this = StringBuilder
                append(" World") 
            }.toString() // 返回 "Hello World"（链式调用关键！）
    
### （4）with: 

        作用域内使用this，返回结果（与run类似但语法不同）

            val sb2 = with(StringBuilder()) {
                append("Hello")
                append(" World")
                toString() // 返回结果
            }
    
### （5）also: 

            作用域内使用it，**返回it**（用于副作用）
            val result = text.also { 
                println("Also: $it") // 输出 "Kotlin"
            }.length // 返回6（与let不同！）

## 三，静态函数（Java互操作）

### （1） Companion Object（类静态成员）

            class Calculator {
                companion object {
                    @JvmStatic // 使方法在Java中可直接调用
                    fun square(n: Int): Int = n * n
                }
            }
            // Java调用：Calculator.square(5)
    
### （2） Object（单例类静态方法）

            object MathUtils {
                @JvmStatic
                fun sqrt(n: Double): Double = Math.sqrt(n)
            }
            // Java调用：MathUtils.sqrt(4.0)

## 四，扩展函数（核心特性）

### （1）扩展函数：为已有类添加方法（不修改原类）

              fun String.addExclamation() = this + "!" // "Hello".addExclamation() -> "Hello!"
    
### （2） 扩展属性（仅限val）

            val String.lengthSquared: Int
                get() = length * length
        
### （3） 重要限制：

        //    - 不能扩展私有成员（如：不能访问类的private属性）
        //    - 不能覆盖已有的方法（会编译错误）
        //    - 仅在导入扩展函数的文件中可用

# 4. 面向对象编程

## 一，类与构造函数：

### 1. 主构造函数（类声明中定义）

    class Person constructor(val name: String, var age: Int) {
        // 次构造函数（必须调用主构造函数）
        constructor(name: String) : this(name, 0) // 默认年龄0
    }

### 2. 初始化块（初始化逻辑）

    class User(val id: Int) {
        init {
            println("User $id created") // 构造时执行
        }
    }
    
### 3. 延迟初始化（lateinit）

    class LazyUser {
        lateinit var name: String // 必须在使用前初始化
        fun initName() { name = "Alice" }
    }

## 二，继承与多态：

### 1. open标记（关键！必须标记父类才能继承）

        open class Animal(val species: String) {
            open fun sound() = "Generic sound" // open标记允许重写
        }
        
### 2. 继承与重写（override）

        class Dog : Animal("Dog") {
            override fun sound() = "Bark!" // 重写父类方法
        }
        
### 3. 密封类（关键：限制继承树）

        sealed class Shape // 仅能被同文件/模块的子类继承
        data class Circle(val radius: Double) : Shape()
        data class Rectangle(val width: Double, val height: Double) : Shape()
        
        // 使用密封类必须覆盖所有子类（编译器强制检查）
        fun getArea(shape: Shape): Double = when (shape) {
            is Circle -> Math.PI * shape.radius * shape.radius
            is Rectangle -> shape.width * shape.height
            // 编译器会报错：when must be exhaustive
        }

## 三，数据类（核心特性）：

### 1. 自动生成：

            equals(), hashCode(), toString(), copy()
             data class User(val id: Int, val name: String)
    
### 2. 使用示例

              val user1 = User(1, "Alice")
              val user2 = User(1, "Alice")
              println(user1 == user2) // true（自动重写equals）
        
              val user3 = user1.copy(name = "Bob") // 复制对象并修改属性
              println(user3) // User(id=1, name=Bob)
        
### 3. 重要限制：

              //    - 主构造函数必须至少有一个参数
              //    - 不能是密封类/抽象类
              //    - 不能有自定义equals()等

## 四，单例类：

### 1. object（单例）

              object Singleton {
               var version = "1.0"
               fun getVersion() = version
             }
    
### 2. 与Companion Object的区别

           class AppConfig {
               companion object { // 类的静态成员（Java中通过AppConfig.getInstance()访问）
                   val API_KEY = "abc123"
                   fun init() { /* ... */ }
               }
           }
           // Java调用：AppConfig.API_KEY
        
### 3. 单例vsCompanion：

           //    - object: 独立类（可单独实例化，但只能有一个实例）
           //    - Companion: 与类绑定（不能单独实例化）
        
## 五，接口（多继承）：

### 1. 基础接口

           interface Logger {
               fun log(message: String)
           }
        
### 2. 实现接口（类可继承多个接口）

           class ConsoleLogger : Logger {
               override fun log(message: String) = println("LOG: $message")
           }
        
### 3. 接口默认实现（Kotlin 1.1+）

           interface Network {
               fun connect() { println("Connecting...") } // 默认实现
           }
        
           class WiFi : Network // 自动继承connect()

# 5. Lambda编程

## 一，基础Lambda

val sum = { a: Int, b: Int -> a + b }
println(sum(2, 3)) // 5

## 二，简写（参数类型可省略，it默认参数）

val double = { x: Int -> x * 2 }
println(double(5)) // 10

## 三，高阶函数（接收Lambda）

fun process(data: List<Int>, transform: (Int) -> Int): List<Int> {
    return data.map(transform)
}
val result = process(listOf(1,2,3), { it * 2 }) // [2,4,6]

## 四，Lambda作为函数参数（省略括号）

fun execute(block: () -> Unit) { block() }
execute { println("Hello") }

# 6. 空指针检查

## 一，安全调用（?.）：避免空指针

val length = user?.name?.length // null if any is null

## 二，Elvis操作符（?:）：提供默认值

val name = user?.name ?: "Unknown"

## 三，非空断言（!!）：危险！仅在确定非空时用

val nonNullName = user!!.name

## 四，安全转换（as?）：类型安全转换

val str: String? = obj as? String

# 7. 泛型

## 一，基础泛型

class Box<T>(val value: T)

## 二，协变（out）：

只读（List<String>是List<Any>的子类）
val list: List<String> = listOf("a")
val anyList: List<Any> = list // ✅

## 三，逆变（in）：

只写（MutableList<String>是MutableList<Any>的父类）
val mutableList: MutableList<Any> = mutableListOf<String>()
mutableList.add("a") // ✅

## 四，上界（T : Comparable<T>）：

约束类型
fun <T : Comparable<T>> max(a: T, b: T) = if (a > b) a else b

# 8. 委托

## 一，委托属性（by）

class User {
    var name: String by Delegates.observable("Default") { _, old, new ->
        println("Name changed: $old -> $new")
    }
}

## 二，内置委托

val lazyValue: String by lazy { "Lazy Value" } // 懒初始化
val observable: String by Observable("Initial") { _, _, new -> println("Changed: $new") }

## 三，自定义委托

class Delegate<T>(var value: T) {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): T = value
    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: T) { this.value = value }
}
var custom: String by Delegate("Custom")

# 9. 协程

## 一，启动协程（默认在Dispatchers.Main）

GlobalScope.launch {
    val result = async { 
        delay(1000) 
        "Result" 
    }.await()
    println(result)
}

挂起函数（suspend）
suspend fun fetchData(): String {
    delay(500)
    return "Data"
}

协程作用域（推荐）
fun main() = runBlocking {
    launch { println("Coroutine 1") }
    launch { println("Coroutine 2") }
}

# 10. 其它补充

## 一，标准库高阶函数

val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 } // [2,4,6]
val filtered = numbers.filter { it > 1 } // [2,3]

## 二，作用域函数（终极简化）

val person = Person("Alice").apply {
    age = 30
    address = "123 Main St"
}

## 三，字符串模板

val name = "Kotlin"
println("Hello, $name!") // Hello, Kotlin!

## 四，@JvmStatic

@ExperimentalCoroutinesApi