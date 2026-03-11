# 一，加载编辑好的布局
    ——————————————————————————————————————————————————————————————
    class FirstActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.first_layout) 
     } 
     
    }
    ——————————————————————————————————————————————————————————————
    FirstActivity 是一个标准的 Android 界面 Activity，通过 onCreate() 加载 first_layout.xml 布局，是 Android 开发中最基础的界面实现方式。

## 1. 类定义与继承

    FirstActivity 是一个 Activity 类（Android 应用的界面组件）。
    : AppCompatActivity 表示继承自 AppCompatActivity（AndroidX 库中的类）。
    它是 Activity 的扩展，提供向后兼容性（支持 Android 5.0+ 的 Material Design 以及 ActionBar 等功能），是现代 Android 开发的推荐写法。

## 2. onCreate() 方法

    这是 Activity 生命周期的起点，系统在创建 Activity 时必须重写此方法。

    super.onCreate(savedInstanceState)   调用父类（AppCompatActivity）的 onCreate
    • 必须先调用，否则 Activity 无法正常初始化
    • savedInstanceState 用于恢复 Activity 的状态（如屏幕旋转后保留数据）

    setContentView(R.layout.first_layout)	加载布局文件
    • R.layout.first_layout 是 资源引用（指向 res/layout/first_layout.xml 文件）
    • 将 XML 布局文件绑定到当前 Activity 的界面

## 3.关键点

    Activity 是 Android 界面的载体，每个界面（如登录页、主页面）都需要一个 Activity 类。
    onCreate() 是入口，系统通过 onCreate() 创建 Activity 并加载界面。
    setContentView() 是关键，没有这行代码，Activity 会没有界面（显示为空白）。

# 二，在Activity中使用Toast
    ——————————————————————————————————————————————————————————————————————————
    override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.first_layout) 
     val button1: Button = findViewById(R.id.button1) 
     button1.setOnClickListener { 
     Toast.makeText(this, "You clicked Button 1", Toast.LENGTH_SHORT).show() 
     } 
    }
    ——————————————————————————————————————————————————————————————————————————
    这段代码展示了 Android Activity 的标准启动流程——加载布局 → 获取控件 → 绑定事件 → 用户交互。理解它，就掌握了 Android UI 开发的基础骨架。
## 1.val button1: Button = findViewById(R.id.button1)

    作用：通过 ID 获取布局中的 Button 控件。
    关键细节：
    R.id.button1：对应布局文件中 Button 的 android:id="@+id/button1"。
    Kotlin 优化：无需 as Button 类型转换（Kotlin 类型推断）。
    ⚠️ 注意：这是旧版写法，现代 Android 开发推荐使用 View Binding（避免 findViewById 的空指针风险）。

## 2.button1.setOnClickListener { ... }

    作用：为按钮设置点击事件监听器。
    Kotlin 语法优势：使用 Lambda 表达式（{ ... }）替代 Java 的匿名内部类，代码更简洁。
    this 的含义：指向当前 Activity（Context），用于创建 Toast。

## 3.Toast.makeText(...).show()
    
    作用：显示轻量级提示（不阻塞 UI，自动消失）。
    参数说明：
    this：上下文（Activity）。
    "You clicked Button 1"：提示文本。
    Toast.LENGTH_SHORT：显示时长（短，约 2 秒）。

## 4.改进示例
    优先使用 View Binding（Android Studio 自动支持），代码更安全简洁。
    ———————————————————————————————————————————————————————————————————————————————————
    private lateinit var binding: ActivityFirstLayoutBinding // 生成的Binding类

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityFirstLayoutBinding.inflate(layoutInflater)
        setContentView(binding.root)
        binding.button1.setOnClickListener { 
            Toast.makeText(this, "You clicked Button 1", Toast.LENGTH_SHORT).show()
        }
    }
    ————————————————————————————————————————————————————————————————————————————————————
    View Binding（视图绑定）
    是什么：一个 Android Jetpack 工具，可以为你的 XML 布局文件自动生成一个绑定类。
    干什么用：让你无需使用 findViewById，就能安全地访问布局文件中的各个 UI 控件（如 TextView, Button 等）。
    好处：代码更简洁、类型安全（不会出现类型转换错误），并且可以避免因控件 ID 在某些布局中不存在而导致的空指针异常。

    ViewModel
    是什么：一个专门用来管理 UI 相关数据的类。
    干什么用：存放和管理 Activity 或 Fragment 所需的数据，并将这些数据提供给 UI。它的生命周期比 Activity/Fragment 更长。
    好处：当设备屏幕旋转导致 Activity 重建时，ViewModel 中的数据不会丢失，从而避免了数据重新加载，提升了用户体验。它是实现 MVVM 架构的关键组件。

# 三，利用menu资源文件，重写onCreateOptionsMenu() 和 onOptionsItemSelected()方法

## 1，重写onCreateOptionsMenu()    
    —————————————————————————————————————————————————————————
    override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    menuInflater.inflate(R.menu.main, menu)
    return true
    }
    ——————————————————————————————————————————————————————————
    “当APP需要显示菜单时，我负责把菜单内容从文件里‘搬’出来，放到屏幕上。”
    就像你开了一家店，需要先挂个“菜单牌”（XML文件），再让店员（menuInflater）把牌挂到门口（menu）。

### 解析

    menuInflater.inflate(R.menu.main, menu)
    R.menu.main：指向你创建的菜单文件（比如 res/menu/main.xml）。
    （就像你写了一个“菜单说明书”，里面写着“添加”“删除”两个按钮）
    menuInflater：Android系统提供的“搬运工”，负责把“说明书”（XML文件）转换成实际可点击的菜单。
    （相当于把“说明书”翻译成APP能看懂的菜单）
    menu：系统给的“空盒子”，用来装新菜单。
    （系统说：“来，把菜单内容塞进这个盒子里！”）
    这行代码的作用：把 main.xml 里的按钮（比如“添加”“删除”）从文件里“搬”到屏幕顶部的菜单里。

    return true
    告诉系统：“我搞定了，这个菜单由我负责”。
    （如果返回 false，系统会忽略这个菜单，顶部就看不到按钮了！）

## 2，重写onOptionsItemSelected()方法
    ————————————————————————————————————————————————————————————————————
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            R.id.add_item -> Toast.makeText(this, "You clicked Add", Toast.LENGTH_SHORT).show()
            R.id.remove_item -> Toast.makeText(this, "You clicked Remove", Toast.LENGTH_SHORT).show()
        }
        return true
    }
    ————————————————————————————————————————————————————————————————————
    “当用户点了菜单里的某个按钮，我负责告诉APP‘做了什么’（比如弹出提示）。”
    没有这行代码，点了菜单按钮只会关闭菜单，不会有任何反应（比如弹出提示）。
    就像顾客点了“添加”按钮，店员必须说“好的，加一个！”（否则顾客以为没反应）。

### 解析

    when (item.itemId)
    item.itemId：当前用户点击的按钮ID（比如“添加”按钮的编号）。
    （就像每个按钮都有个“身份证号”，系统通过这个号知道用户点了哪个）
    R.id.add_item：在 main.xml 里定义的“添加”按钮的ID（比如 <item android:id="@+id/add_item" ...>）。

    R.id.add_item -> Toast...
    如果用户点了“添加”按钮（R.id.add_item），就执行：
    Toast.makeText(this, "You clicked Add", Toast.LENGTH_SHORT).show()
    （弹出一个短提示：“You clicked Add”）
    同理，“删除”按钮（R.id.remove_item）会弹出“You clicked Remove”。

    return true
    告诉系统：“这个按钮我处理完了，不用再执行默认操作”。
    （如果返回 false，点击后菜单可能自动关闭，但不会执行提示！）

# 四，编写显示Intent代码实现Activity穿梭
    ——————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val intent = Intent(this, SecondActivity::class.java) 
     startActivity(intent) 
    } 
    ——————————————————————————————————————————————————————————

# 五，编写隐式Intent代码并添加category
    ——————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val intent = Intent("com.example.activitytest.ACTION_START") 
     intent.addCategory("com.example.activitytest.MY_CATEGORY") 
     startActivity(intent) 
    } 
    ——————————————————————————————————————————————————————————
## 解释

    可以看到，我们使用了Intent的另一个构造函数，直接将action的字符串传了进去，表明我们
    想要启动能够响应com.example.activitytest.ACTION_START这个action的Activity。

    android.intent.category.DEFAULT是一种默认的category，
    在调用startActivity()方法的时候会自动将这个category添加到Intent中。
    
    可以调用Intent中的addCategory()方法来添加一个category，这里我们指定了一个自定义
    的category，值为com.example.activitytest.MY_CATEGORY。

# 六，编写隐式Intent来启动网页
    ——————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val intent = Intent(Intent.ACTION_VIEW) 
     intent.data = Uri.parse("https://www.baidu.com") 
     startActivity(intent) 
    } 
    ——————————————————————————————————————————————————————————
## 解释

    这里我们首先指定了Intent的action是Intent.ACTION_VIEW，这是一个Android系统内置
    的动作，其常量值为android.intent.action.VIEW。然后通过Uri.parse()方法将一个
    网址字符串解析成一个Uri对象，再调用Intent的setData()方法将这个Uri对象传递进去。
    当然，这里再次使用了前面学习的语法糖，看上去像是给Intent的data属性赋值一样。
    重新运行程序，在FirstActivity界面点击按钮就可以看到打开了系统浏览器

# 七，用Intent传递数据示例

## 1，传入示例

    ————————————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val data = "Hello SecondActivity" 
     val intent = Intent(this, SecondActivity::class.java) 
     intent.putExtra("extra_data", data) 
     startActivity(intent) 
    } 
    ————————————————————————————————————————————————————————————————
    putExtra()方法接收两个参数，第一个参数是键，用于之后从
    Intent中取值，第二个参数才是真正要传递的数据。

## 2，接收示例

    ——————————————————————————————————————————————————————————————————
    class SecondActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.second_layout) 
     val extraData = intent.getStringExtra("extra_data") 
     Log.d("SecondActivity", "extra data is $extraData") 
     } 
     
    }
    ——————————————————————————————————————————————————————————————————
    上述代码中的intent实际上调用的是父类的getIntent()方法，该方法会获取用于启动
    SecondActivity的Intent，然后调用getStringExtra()方法并传入相应的键值，就可以得
    到传递的数据了。这里由于我们传递的是字符串，所以使用getStringExtra()方法来获取传
    递的数据。如果传递的是整型数据，则使用getIntExtra()方法；如果传递的是布尔型数据，
    则使用getBooleanExtra()方法，以此类推。、

# 八，返回数据给上一个Activity

## 1，修改FirstActivity中按钮的点击事件
    ——————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val intent = Intent(this, SecondActivity::class.java) 
     startActivityForResult(intent, 1) 
    }
    ——————————————————————————————————————————————————————
    startActivityForResult()方法接收两个参数：第一个参数还是Intent；第二个参数是请
    求码，用于在之后的回调中判断数据的来源。
    这里我们使用了startActivityForResult()方法来启动SecondActivity，请求码只要是一
    个唯一值即可，这里传入了1。接下来我们在SecondActivity中给按钮注册点击事件，并在点
    击事件中添加返回数据的逻辑

## 2，SecondActivity中给按钮注册点击事件，并在点击事件中添加返回数据的逻辑
    ———————————————————————————————————————————————————————
    class SecondActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.second_layout) 
     button2.setOnClickListener { 
     val intent = Intent() 
     intent.putExtra("data_return", "Hello FirstActivity") 
     setResult(RESULT_OK, intent) 
     finish() 
     } 
     } 
     
    } 
    ———————————————————————————————————————————————————————
    可以看到，我们还是构建了一个Intent，只不过这个Intent仅仅用于传递数据而已，它没有指定
    任何的“意图”。紧接着把要传递的数据存放在Intent中，然后调用了setResult()方法。这个
    方法非常重要，专门用于向上一个Activity返回数据。setResult()方法接收两个参数：第一
    个参数用于向上一个Activity返回处理结果，一般只使用RESULT_OK或RESULT_CANCELED这
    两个值；第二个参数则把带有数据的Intent传递回去。最后调用了finish()方法来销毁当前
    Activity。

## 3，在FirstActivity中重写onActivityResult()方法
    ————————————————————————————————————————————————————————
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) { 
     super.onActivityResult(requestCode, resultCode, data) 
     when (requestCode) { 
     1 -> if (resultCode == RESULT_OK) { 
     val returnedData = data?.getStringExtra("data_return") 
     Log.d("FirstActivity", "returned data is $returnedData") 
     } 
     } 
    } 
    ————————————————————————————————————————————————————————
    由于我们是使用startActivityForResult()方法来启动SecondActivity的，在
    SecondActivity被销毁之后会回调上一个Activity的onActivityResult()方法，需重写，

    onActivityResult()方法带有3个参数：第一个参数requestCode，即我们在启动Activity
    时传入的请求码；第二个参数resultCode，即我们在返回数据时传入的处理结果；第三个参
    数data，即携带着返回数据的Intent。由于在一个Activity中有可能调用
    startActivityForResult()方法去启动很多不同的Activity，每一个Activity返回的数据都
    会回调到onActivityResult()这个方法中，因此我们首先要做的就是通过检查
    requestCode的值来判断数据来源。确定数据是从SecondActivity返回的之后，我们再通过
    resultCode的值来判断处理结果是否成功。最后从data中取值并打印出来，这样就完成了向
    上一个Activity返回数据的工作。

    重新运行程序，在FirstActivity的界面点击按钮会打开SecondActivity，然后在
    SecondActivity界面点击Button 2按钮会回到FirstActivity，这时查看Logcat的打印信息

## 4，重写onBackPressed()方法
    ————————————————————————————————————————————————————————
    override fun onBackPressed() { 
     val intent = Intent() 
     intent.putExtra("data_return", "Hello FirstActivity") 
     setResult(RESULT_OK, intent) 
     finish() 
    }
    ————————————————————————————————————————————————————————
    当用户按下Back键后，就会执行onBackPressed()方法中的代码，我们在这里添加返
    回数据的逻辑就行了。避免了没有数据返回的情况发生

# 九，Activity类中定义了7个回调方法和三种生存期

## 七个回调方法

### 1 onCreate()。

    这个方法你已经看到过很多次了，我们在每个Activity中都重写了这个方
    法，它会在Activity第一次被创建的时候调用。你应该在这个方法中完成Activity的初始化
    操作，比如加载布局、绑定事件等。

### 2 onStart()。

    这个方法在Activity由不可见变为可见的时候调用。

### 3 onResume()。

    这个方法在Activity准备好和用户进行交互的时候调用。此时的Activity一
    定位于返回栈的栈顶，并且处于运行状态。

### 4 onPause()。

    这个方法在系统准备去启动或者恢复另一个Activity的时候调用。我们通常
    会在这个方法中将一些消耗CPU的资源释放掉，以及保存一些关键数据，但这个方法的执
    行速度一定要快，不然会影响到新的栈顶Activity的使用。

### 5 onStop()。

    这个方法在Activity完全不可见的时候调用。它和onPause()方法的主要区
    别在于，如果启动的新Activity是一个对话框式的Activity，那么onPause()方法会得到执
    行，而onStop()方法并不会执行。

### 6 onDestroy()。

    这个方法在Activity被销毁之前调用，之后Activity的状态将变为销毁状态。

### 7 onRestart()。

    这个方法在Activity由停止状态变为运行状态之前调用，也就是Activity
    被重新启动了。

## 三种生存期

    完整生存期。Activity在onCreate()方法和onDestroy()方法之间所经历的就是完整生
    存期。一般情况下，一个Activity会在onCreate()方法中完成各种初始化操作，而在
    onDestroy()方法中完成释放内存的操作。

    可见生存期。Activity在onStart()方法和onStop()方法之间所经历的就是可见生存
    期。在可见生存期内，Activity对于用户总是可见的，即便有可能无法和用户进行交互。我
    们可以通过这两个方法合理地管理那些对用户可见的资源。比如在onStart()方法中对资
    源进行加载，而在onStop()方法中对资源进行释放，从而保证处于停止状态的Activity不
    会占用过多内存。

    前台生存期。Activity在onResume()方法和onPause()方法之间所经历的就是前台生存
    期。在前台生存期内，Activity总是处于运行状态，此时的Activity是可以和用户进行交互
    的，我们平时看到和接触最多的就是这个状态下的Activity。

# 十，体验完整的Activity生命周期
    ————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     private val tag = "MainActivity" 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     Log.d(tag, "onCreate") 
     setContentView(R.layout.activity_main) 
     startNormalActivity.setOnClickListener { 
     val intent = Intent(this, NormalActivity::class.java) 
     startActivity(intent) 
     } 
     startDialogActivity.setOnClickListener { 
     val intent = Intent(this, DialogActivity::class.java) 
     startActivity(intent) 
     } 
     } 
     
     override fun onStart() { 
     super.onStart() 
     Log.d(tag, "onStart") 
     } 
     
     override fun onResume() { 
     super.onResume() 
     Log.d(tag, "onResume") 
     } 
     
     override fun onPause() { 
     super.onPause() 
     Log.d(tag, "onPause") 
     } 
     
     override fun onStop() { 
     super.onStop() 
     Log.d(tag, "onStop") 
     } 
     
     override fun onDestroy() { 
     super.onDestroy() 
     Log.d(tag, "onDestroy") 
     } 
     
     override fun onRestart() { 
     super.onRestart() 
     Log.d(tag, "onRestart") 
     } 
     
    }
    ————————————————————————————————————————————————————————

# 十一，临时数据保存与恢复

## 临时数据保存
    ——————————————————————————————————————————————————————
    override fun onSaveInstanceState(outState: Bundle) { 
     super.onSaveInstanceState(outState) 
     val tempData = "Something you just typed" 
     outState.putString("data_key", tempData) 
    } 
    ——————————————————————————————————————————————————————
### 解释
    onSaveInstanceState()回调方法，这个方法可以保证在Activity被回收之前一定会被调用
    onSaveInstanceState()方法会携带一个Bundle类型的参数，Bundle提供了一系列的方法
    用于保存数据，比如可以使用putString()方法保存字符串，使用putInt()方法保存整型数
    据，以此类推。每个保存方法需要传入两个参数，第一个参数是键，用于后面从Bundle中取
    值，第二个参数是真正要保存的内容。

## 数据恢复
    ————————————————————————————————————————————————————————
    override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     Log.d(tag, "onCreate") 
     setContentView(R.layout.activity_main) 
     if (savedInstanceState != null) { 
     val tempData = savedInstanceState.getString("data_key") 
     Log.d(tag, "tempData is $tempData") 
     } 
     ... 
    }
    ————————————————————————————————————————————————————————
    我们一直使用的onCreate()方法其实也有一个Bundle类型的参数。这个参数在一般情况下都是
    null，但是如果在Activity被系统回收之前，你通过onSaveInstanceState()方法保存数
    据，这个参数就会带有之前保存的全部数据，我们只需要再通过相应的取值方法将数据取出即可。

# 十二，