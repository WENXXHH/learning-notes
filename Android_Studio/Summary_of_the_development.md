# 《一》先从看得见的入手，探究Activity

## 一，创建简单Activity的完整过程

### 1，

    在project的模式下，鼠标右击app/src/main/java/com.example.可以完成新建......

### 2，

    新建Activity之后，右击app/src/main/res目录->New->Directory,创建一个名为layout的目录右击layout目录，
    New->Layout resource file,随意命名，根目录默认，可以在编辑界面找到有图片的图像，点击想要的控件添加即可

### 3，

    编辑layout.xml文件，
    之后在Activity文件中的onCreate（）方法中编写setContentView(R.layout.xml文件名）

### 4.

    在AndroidManifest文件里面进行主Activity的声明，请注意以下几点：找到对应的Activity标签，修改exported="true"，
    以及移除 activity 标签的自闭合符 “ / ”，就在下面添加如下的代码：
    ——————————————————————————————————————————————————————
    <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    ——————————————————————————————————————————————————————
    最后点击编译就可以完成了
    销毁当前activity的方法就是按键盘上的Esc键

## 二，使用Toast

### 1，创键button
    先完成创建一个完整的Activity，为了让按钮可以被正常点击，可以采用使按钮处于布局的中央位置
    修改first_layout.xml，在根目录添加  android:gravity="center"
    或者现在可以先取消标题栏，选择在Button按钮处修改  android:layout_marginTop="80dp"

### 2,编辑点击事件
    修改对应Activity的onCreate（），在里面添加按钮的点击事件，通过代码补全添加以下代码
    ————————————————————————————————————————————————————————————
    val button1: Button = findViewById(R.id.button1) 
     button1.setOnClickListener { 
     Toast.makeText(this, "You clicked Button 1", Toast.LENGTH_SHORT).show() 
     } 
    ————————————————————————————————————————————————————————————
    再启动程序点击按钮就可以实现了

## 三，使用Menu菜单

### 1，创建menu布局
    首先在res目录下新建一个menu文件夹，右击res目录→New→Directory，输入文件夹
    名“menu”，点击“OK”。接着在这个文件夹下新建一个名叫“main”的菜单文件，右击menu文
    件夹→New→Menu resource ﬁle
    接下来就可以在里面编辑内容了，例如，可以添加以下两个选项的代码
    ————————————————————————————————————————————————————————————————
    <item 
     android:id="@+id/add_item" 
     android:title="Add"/> 
    <item 
     android:id="@+id/remove_item" 
     android:title="Remove"/>
    ————————————————————————————————————————————————————————————————

### 2，引入布局
    接着回到FirstActivity中来重写onCreateOptionsMenu()方法，
    重写方法可以使用Ctrl + O 快捷键（Mac系统是control + O）
    ————————————————————————————————————————————————————————————————
     override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        menuInflater.inflate(R.menu.main, menu)
        return true
        }
    ————————————————————————————————————————————————————————————————

### 3，编辑点击Item事件
    最后在FirstActivity中重写onOptionsItemSelected()方法
    ————————————————————————————————————————————————————————————————————
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            R.id.add_item -> 
            Toast.makeText(this, "You clicked Add", Toast.LENGTH_SHORT).show()
            R.id.remove_item -> 
            Toast.makeText(this, "You clicked Remove", Toast.LENGTH_SHORT).show()
        }
        return true
    }
    ————————————————————————————————————————————————————————————————————

## 四，初步练习Intent

### 1，（通用）创建布局
    在已经创建了一个完整的Activity的基础上，
    还是右击com.example.activitytest包→New→Activity→Empty Activity，会弹出一个创建
    Activity的对话框，这次我们命名为SecondActivity，并勾选Generate Layout File，给布局
    文件起名为second_layout，但不要勾选Launcher Activity选项
    只是练习就可以按照first_layout的布局编写新的布局就可以了，
    注意删去SecondActivity的多余自生成部分，留下表示引入布局的部分就可以了

### 2，（显式）点击事件触发Intent
    修改FirstAcivity文件里面的setOnClickListener（）
     ——————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val intent = Intent(this, SecondActivity::class.java) 
     startActivity(intent) 
    } 
    ——————————————————————————————————————————————————————————
    显式到这里就算完成了

### 3，（隐式）添加可以响应的category
    接下来是完成隐式编写
    先完成第一步，然后打开AndroidManifest.xml，添加如下代码：
    ——————————————————————————————————————————————————————————————————————————
    <activity android:name=".SecondActivity" > 
     <intent-filter> 
     <action android:name="com.example.activitytest.ACTION_START" /> 
     <category android:name="android.intent.category.DEFAULT" />
     <category android:name="com.example.activitytest.MY_CATEGORY"/> //添加这行
     </intent-filter> 
    </activity>
    ——————————————————————————————————————————————————————————————————————————
    还要注意将false修改成true

### 4，（隐式）指定action,编辑category
     修改FirstActivity中按钮的点击事件，代码如下所示：
    ——————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val intent = Intent("com.example.activitytest.ACTION_START") 
     intent.addCategory("com.example.activitytest.MY_CATEGORY") 
     startActivity(intent) 
    } 
    ——————————————————————————————————————————————————————————
    这里还拓展了增加category的写法，

### 5，（跳转网页）
    无需前面的步骤，直接在FirstActivity中操作
    修改按钮点击事件
    ——————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val intent = Intent(Intent.ACTION_VIEW) 
     intent.data = Uri.parse("https://www.baidu.com") 
     startActivity(intent) 
    } 
    ——————————————————————————————————————————————————————————

### 6，（拓展）
    承接第五步
    右击com.example.activitytest包→New→Activity→Empty Activity，新建ThirdActivity，
    并勾选Generate Layout File，给布局文件起名为third_layout，点击“Finish”完成创建。然
    后编辑third_layout.xml，只是练习就可以按照first_layout的布局编写新的布局就可以了，
    注意删去ThirdActivity的多余自生成部分，留下表示引入布局的部分就可以了

### 7，（拓展）验证隐式Intent的响应条件
    在AndroidManifest.xml中修改ThirdActivity的注册信息：
    ————————————————————————————————————————————————————————————    
    <activity android:name=".ThirdActivity"> 
     <intent-filter tools:ignore="AppLinkUrlError"> 
     <action android:name="android.intent.action.VIEW" /> 
     <category android:name="android.intent.category.DEFAULT" /> 
     <data android:scheme="https" /> 
     </intent-filter> 
    </activity>
    ————————————————————————————————————————————————————————————    
    貌似没成功，以后再探讨，作用不大

### 8，（跳转电话）
     无需前面的步骤，直接在FirstActivity中操作
    修改按钮点击事件
    ——————————————————————————————————————————————————————————
    button1.setOnClickListener {
            val intent = Intent(Intent.ACTION_DIAL)
            intent.data = Uri.parse("tel:10086") 
                    startActivity(intent)
        }
    ——————————————————————————————————————————————————————————

## 五，向下一个Activity传递数据

### 1，编辑点击事件传递数据
    创建好两个Activity之后，先编辑FirstActivity，添加以下的代码
     ————————————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val data = "Hello SecondActivity" 
     val intent = Intent(this, SecondActivity::class.java) 
     intent.putExtra("extra_data", data) //关键是这一行 
     startActivity(intent) 
    } 
    ————————————————————————————————————————————————————————————————

### 2，获取键值对数据
    然后在SecondActivity中将传递的数据取出，并打印出来，代码如下所示：
    ——————————————————————————————————————————————————————————————————
    class SecondActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.second_layout) 
     val extraData = intent.getStringExtra("extra_data") //获取键值对
     Log.d("SecondActivity", "extra data is $extraData") //可视化处理
     } 
     
    }
    ——————————————————————————————————————————————————————————————————
    运行程序然后查看Logcat就可以查看到想要的信息了

## 六，返回数据给上一个Activity

    创建好两个Activity之后
### 1，修改FirstActivity中按钮的点击事件
    ——————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val intent = Intent(this, SecondActivity::class.java) 
     startActivityForResult(intent, 1) 
    }
    ——————————————————————————————————————————————————————

### 2，构建Intent返回数据
    SecondActivity中给按钮注册点击事件，并在点击事件中添加返回数据的逻辑
    ———————————————————————————————————————————————————————
    class SecondActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.second_layout)
            val button2: Button = findViewById(R.id.button2)
            button2.setOnClickListener {
                val intent = Intent()
                intent.putExtra("data_return", "Hello FirstActivity") //还是键值对
                setResult(RESULT_OK, intent)
                finish()
            }
        }
    }
    ———————————————————————————————————————————————————————

### 3，在FirstActivity中重写onActivityResult()方法
    与之前学习的传递数据的思路类似
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

### 4，考虑”back"的情况
    在SecondActivity的onCreate()添加onBackPressedDispatcher方法
    代码与第二步是保持一致的
    为了确保按下“back"销毁了Activity也可以实现传递数据
    ————————————————————————————————————————————————————————
     setContentView(R.layout.second_layout)
        onBackPressedDispatcher.addCallback(this) {
            val intent = Intent()
            intent.putExtra("data_return", "Hello FirstActivity")
            setResult(RESULT_OK, intent)
            finish()
        }
        val button2: Button = findViewById(R.id.button2)
    ————————————————————————————————————————————————————————

## 七，体验完整的Activity生命周期

### 1，创建必要文件
    先创建一个新项目，允许系统自己创建Activity和布局
    再右击com.example.activitylifecycletest包→New→Activity→Empty Activity，新建
    NormalActivity，布局起名为normal_layout。然后使用同样的方式创建DialogActivity，布
    局起名为dialog_layout。

### 2，编辑两个实验布局
    先修改normal_layout.xml,添加以下代码为例
    ——————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <TextView 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="This is a normal activity" 
     /> 
     
    </LinearLayout>
    ——————————————————————————————————————————————————————————
    然后编辑dialog_layout.xml文件，将里面的代码替换成如下内容：
    ——————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <TextView 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="This is a dialog activity" 
     /> 
     
    </LinearLayout>
    ——————————————————————————————————————————————————————————

### 3，注册编辑
    修改AndroidManifest.xml的<activity>标签的配置，就是添加一行而已
    应该还记得需要将false修改成true，如下所示：
    ————————————————————————————————————————————————————————————————
    <activity android:name=".DialogActivity" 
     android:theme="@style/Theme.AppCompat.Dialog"> //添加了这一行
    </activity> 
    <activity android:name=".NormalActivity"> 
    </activity>
    ————————————————————————————————————————————————————————————————

### 4，编辑主布局
    接下来我们修改activity_main.xml，重新定制主Activity的布局，将里面的代码替换成如下内容：
    ————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent">
     

     <Button 
     android:id="@+id/startNormalActivity" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Start NormalActivity" 
     android:layout_marginTop="80dp"/> 
    
     
     <Button 
     android:id="@+id/startDialogActivity" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Start DialogActivity" /> 
     
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————

### 5，重写各回调方法打印可视化
    最后修改MainActivity中的代码，如下所示：
    ——————————————————————————————————————————————————————————————————————————————
    class MainActivity : ComponentActivity() {
        private val tag = "MainActivity"
    
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            Log.d(tag, "onCreate") //第一个打印
            setContentView(R.layout.activity_main)
            val startNormalActivity: Button = findViewById(R.id.startNormalActivity)
            val startDialogActivity: Button = findViewById(R.id.startDialogActivity)
    
            startNormalActivity.setOnClickListener {
                val intent = Intent(this, NormalActivity::class.java)
                startActivity(intent)
            }
            startDialogActivity.setOnClickListener {
                val intent = Intent(this, DialogActivity::class.java)
                startActivity(intent)
            }
        }
    
        private fun setOnClickListener(function: () -> Unit) {}
    
        override fun onStart() {
            super.onStart()
            Log.d(tag, "onStart") //第二次打印
        }
    
        override fun onResume() {
            super.onResume()
            Log.d(tag, "onResume") //3
        }
    
        override fun onPause() {
            super.onPause()
            Log.d(tag, "onPause") //4
        }
    
        override fun onStop() {
            super.onStop()
            Log.d(tag, "onStop") //5
        }
    
        override fun onDestroy() {
            super.onDestroy()
            Log.d(tag, "onDestroy") //6
        }
    
        override fun onRestart() {
            super.onRestart()
            Log.d(tag, "onRestart") //7
        }
        }
    ——————————————————————————————————————————————————————————————————————————————

## 八，数据的临时保存与恢复

### 1，重写onSaveInstanceState()
    先至少完成创造了一个完整的Activity的过程，
    在MainActivity中添加如下代码
    ————————————————————————————————————————————————————
    override fun onSaveInstanceState(outState: Bundle) { 
     super.onSaveInstanceState(outState) 
     val tempData = "Something you just typed" 
     outState.putString("data_key", tempData) 
    }
    ————————————————————————————————————————————————————

### 2，获取保存的数据
    然后修改MainActivity的onCreate()方法，如下所示：
    —————————————————————————————————————————————————————
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
    —————————————————————————————————————————————————————

## 九，四种启动方式练习
    等项目驱动，平常就使用默认的standard就可以了

## 十，知晓当前是在哪一个Activity

### 1，创建BaseActivity
    我们还是在ActivityTest项目的基础上修改，首先需要新建一个BaseActivity类。右击
    com.example.activitytest包→New→Kotlin File/Class，在弹出的窗口中输入
    BaseActivity，创建类型选择Class
    如下所示：
    ————————————————————————————————————————————————————————
    open class BaseActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     Log.d("BaseActivity", javaClass.simpleName) //由此判断当前
     } 
     
    }
    —————————————————————————————————————————————————————————
    将 BaseActivity作为父类 ， 这样就可以在logcat里面查看当前是属于哪一个Activity了

### 2，
    然后修改
    FirstActivity、SecondActivity和ThirdActivity的继承结构，让它们不再继承自
    AppCompatActivity，而是继承自BaseActivity。

    而由于BaseActivity又是继承自AppCompatActivity的，
    所以项目中所有Activity的现有功能并不受影响，它们仍然继承了Activity中的所有特性。

## 十一，随时随地退出程序

### 1，新建一个单例类
    新建一个单例类ActivityCollector作为Activity的集合，代码如下所示：
    ————————————————————————————————————————————————————————
    object ActivityCollector { 
     
     private val activities = ArrayList<Activity>() 
     
     fun addActivity(activity: Activity) { 
     activities.add(activity) 
     } 
     
     fun removeActivity(activity: Activity) { 
     activities.remove(activity) 
     } 
     
     fun finishAll() { 
     for (activity in activities) { 
     if (!activity.isFinishing) { 
     activity.finish() 
     } 
     } 
     activities.clear() 
     } 
     
    } 
    ————————————————————————————————————————————————————————

### 2，在父类中添加单例类方法
    接下来修改BaseActivity中的代码，如下所示：
    ————————————————————————————————————————————————————————
    open class BaseActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     Log.d("BaseActivity", javaClass.simpleName) 
     ActivityCollector.addActivity(this) 
     } 
     
     override fun onDestroy() { 
     super.onDestroy() 
     ActivityCollector.removeActivity(this) 
     } 
     
    }
    ————————————————————————————————————————————————————————

### 3，使用示例
    不管你想在什么地方退出程序，只需要调用ActivityCollector.finishAll()
    方法就可以了。例如在ThirdActivity界面想通过点击按钮直接退出程序，只需将代码改成如下形式：
    ——————————————————————————————————————————————————————————
    class ThirdActivity : BaseActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     Log.d("ThirdActivity", "Task id is $taskId") 
     setContentView(R.layout.third_layout) 
     button3.setOnClickListener { 
     ActivityCollector.finishAll() 
     } 
     } 
     
    }
    ——————————————————————————————————————————————————————————
    当然你还可以在销毁所有Activity的代码后面再加上杀掉当前进程的代码，以保证程序完全退
    出，杀掉进程的代码如下所示：
    android.os.Process.killProcess(android.os.Process.myPid()) 

## 十二，启动Activity的最佳写法

### 1,添加companion object
    修改SecondActivity中的代码，如下所示：
    ————————————————————————————————————————————————————————————————————
    class SecondActivity : BaseActivity() { 
     ... 
     companion object { 
     fun actionStart(context: Context, data1: String, data2: String) { 
     val intent = Intent(context, SecondActivity::class.java) 
     intent.putExtra("param1", data1) 
     intent.putExtra("param2", data2) 
     context.startActivity(intent) 
     } 
     } 
    }
    —————————————————————————————————————————————————————————————————————

### 2，上一级启动简化
    只需要一行代码就可以启动SecondActivity，如下所示：
    ————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     SecondActivity.actionStart(this, "data1", "data2") 
    } 
    ————————————————————————————————————————————————————————
    记住这是一个需要保持的良好的编程习惯就行了

# 《二》UI开发的点点滴滴

## 一，使用实现接口的方式来进行注册button点击事件，并通过点击按钮获取EditText中输入的内容。

### 1，添加EditText
    先在一个创建好的布局文件里面添加EditText控件
    ——————————————————————————————————————————————————————————
    <EditText 
     android:id="@+id/editText" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:hint="Type something here" 
     android:maxLines="2" 
     />
    ——————————————————————————————————————————————————————————

### 2，添加获取内容的事件
    然后修改对应的Activity中的编辑button点击事件的代码
    ————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity(), View.OnClickListener { 
     
     override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.first_layout)
        val button1: Button = findViewById(R.id.button1)
        button1.setOnClickListener(this)
    }
     
     //获取EditText输入的内容
     override fun onClick(v: View?) { 
     when (v?.id) { 
     R.id.button -> { 
     val editText: EditText = findViewById(R.id.editText)
     val inputText = editText.text.toString() 
     Toast.makeText(this, inputText, Toast.LENGTH_SHORT).show() 
     } 
     } 
     } 
     
    } 
    ————————————————————————————————————————————————————————————

## 二，使用ImageView

### 1,添加Imageview
    我们在res目录下再新建一个drawable-xxhdpi目录，然后将事先准备好的两张图片img_1.png和img_2.png复制到该目录当中。
    接下来修改activity_main.xml，添加如下代码：
    ——————————————————————————————————————————————————————————————
    <ImageView 
     android:id="@+id/imageView" 
     android:layout_width="wrap_content" 
     android:layout_height="200dp" 
     android:src="@drawable/img_1" 
     android:scaleType="centerInside" //看看有没有作用
     /> 
    ——————————————————————————————————————————————————————————————

### 2,可选动态改变图片
    如果想要动态地更改ImageView中的图片，就修改Activity的代码
    ——————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity(), View.OnClickListener { 
     
     ... 
     
     override fun onClick(v: View?) { 
     when (v?.id) { 
     R.id.button -> { 
     val imageView: ImageView = findViewById(R.id.imageView)
     imageView.setImageResource(R.drawable.img_2)
     } 
     } 
     } 
     
    }
    ——————————————————————————————————————————————————————————————

## 三，使用ProgressBar进度条

### 1，布局
    先在布局中添加你想要的版本，以下是两个版本
    圆形进度条
    ————————————————————————————————————————————
    <ProgressBar 
     android:id="@+id/progressBar" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     /> 
    ————————————————————————————————————————————

    水平进度条
    —————————————————————————————————————————————
    <ProgressBar 
     android:id="@+id/progressBar" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     style="?android:attr/progressBarStyleHorizontal" 
     android:max="100" 
     />
    —————————————————————————————————————————————

### 2，点击实现消失和出现
    如果想要实现点击一下按钮让进度条消失，再点击一下按钮让进度条出现。修改Activity中的代码，如下所示：
    ——————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity(), View.OnClickListener { 
     
     ... 
     
     override fun onClick(v: View?) { 
     when (v?.id) { 
     R.id.button -> { 
     if (progressBar.visibility == View.VISIBLE) { 
     progressBar.visibility = View.GONE 
     } else { 
     progressBar.visibility = View.VISIBLE 
     }                  //选择结构实现两种形态的转变
     } 
     } 
     } 
     
    } 
    ————————————————————————————————————————————————————————————————————

### 3，水平进度条动态增加
    动态地更改水平进度条的进度，可以修改Activity中的代码如下所示：
    ————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity(), View.OnClickListener { 
     
     ... 
     
     override fun onClick(v: View?) { 
     when (v?.id) { 
     R.id.button -> { 
     progressBar.progress = progressBar.progress + 10 
     } 
     } 
     } 
     
    }
    ————————————————————————————————————————————————————————————————————————

## 四，使用AlertDialog确认框

### 1，点击事件生成对话框
    编辑Activity中的代码
     ————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity(), View.OnClickListener { 
     ... 
     override fun onClick(v: View?) { 
     when (v?.id) { 
     R.id.button -> { 
     AlertDialog.Builder(this).apply { 
     setTitle("This is Dialog") 
     setMessage("Something important.") 
     setCancelable(false) 
     setPositiveButton("OK") { dialog, which -> 
     } 
     setNegativeButton("Cancel") { dialog, which -> 
     } 
     show() 
     } 
     } 
     } 
     } 
     
    }
    ————————————————————————————————————————————————————

## 五，引入自定义的标题栏布局
    由于没有找到随书的图片资源，操作失败
### 1，创建自定义布局
    这里我提前准备好了3张图片——title_bg.png、back_bg.png和edit_bg.png（资源下载地址见
    前言），分别用于作为标题栏、返回按钮和编辑按钮的背景。
    先在layout目录下创建一个title.xml布局，填写以下代码
    ————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     //android:background="@drawable/title_bg"> 
     
     <Button
     android:id="@+id/titleBack" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center" 
     android:layout_margin="5dp" 
     //android:background="@drawable/back_bg" 
     android:text="Back" 
     android:textColor="#fff" /> 
     
     <TextView 
     android:id="@+id/titleText" 
     android:layout_width="0dp" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center" 
     android:layout_weight="1" 
     android:gravity="center" 
     android:text="Title Text" 
     android:textColor="#fff" 
     android:textSize="24sp" /> 
     
     <Button 
     android:id="@+id/titleEdit" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center" 
     android:layout_margin="5dp" 
     //android:background="@drawable/edit_bg" 
     android:text="Edit" 
     android:textColor="#fff" /> 
     
    </LinearLayout> 

    ————————————————————————————————————————————————————————————

### 2，主布局中引入自定义
    接着修改activity_main.xml中的代码，如下所示：
    ————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <include layout="@layout/title" /> //一句引入
     
    </LinearLayout> 
    ——————————————————————————————————————————————————————————————————————————

### 3，可选，似乎有替代方案
    最后在MainActivity中将系统自带的标题栏隐藏掉，代码如下所示：
    ——————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     supportActionBar?.hide() 
     } 
     
    }
    ——————————————————————————————————————————————————————————

## 六，创建自定义控件

### 1，新建TitleLayout类
    新建TitleLayout继承自LinearLayout，让它成为我们自定义的标题栏控件，代码如下所示：
    ——————————————————————————————————————————————————————————————————————————————————————————
    class TitleLayout(context: Context, attrs: AttributeSet) : LinearLayout(context, attrs) { 
     
     init { 
     LayoutInflater.from(context).inflate(R.layout.title, this) //还是使用上面创建的自定义标题栏
     } 
     
    }
    ————————————————————————————————————————————————————————————————————————————————————————————

### 2，主布局中添加
    接下来我们需要在布局文件中添加这个自定义控件，修改activity_main.xml中的代码，如下所示：
    ——————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
    
     //添加的自定义布局，引入自定义标题栏
     <com.example.uicustomviews.TitleLayout 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" />

    </LinearLayout> 
    ——————————————————————————————————————————————————————————————————————

### 3，统一编辑标题栏的点击事件
    下面我们尝试为标题栏中的按钮注册点击事件，修改TitleLayout中的代码，如下所示：
     ——————————————————————————————————————————————————————————————————————————————————————————————
    class TitleLayout(context: Context, attrs: AttributeSet) : LinearLayout(context, attrs) { 
     
     init { 
     LayoutInflater.from(context).inflate(R.layout.title, this) 
     titleBack.setOnClickListener { 
     val activity = context as Activity 
     activity.finish() 
     } 
     titleEdit.setOnClickListener { 
     Toast.makeText(context, "You clicked Edit button", Toast.LENGTH_SHORT).show() 
     } 
     } 
     
    }
    ——————————————————————————————————————————————————————————————————————————————————————————————

## 七，简单使用ListView

### 1,添加ListView布局
    创建一个新项目，先修改 activity_main.xml中的代码
    ————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <ListView 
     android:id="@+id/listView" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" /> 
     
    </LinearLayout>
    ——————————————————————————————————————————————————————————————————————————

### 2,数据与适配器设置
    接下来修改MainActivity中的代码
    ———————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     private val data = listOf("Apple", "Banana", "Orange", "Watermelon", 
     "Pear", "Grape", "Pineapple", "Strawberry", "Cherry", "Mango", 
     "Apple", "Banana", "Orange", "Watermelon", "Pear", "Grape", 
     "Pineapple", "Strawberry", "Cherry", "Mango") 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val adapter = ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,data) 
     listView.adapter = adapter 
     } 
     
    } 
    ———————————————————————————————————————————————————————————————

## 八，定制fruit_ListView的界面（懵懂）
    缺点还是找不到合适的图片
### 1，新建类
    首先需要准备好一组图片资源（资源下载地址见前言），分别对应上面提供的每一种水果，
    待会我们要让这些水果名称的旁边都有一张相应的图片。
    接着定义一个实体类，作为ListView适配器的适配类型。新建Fruit类，代码如下所示：
    ————————————————————————————————————————————————————————————————————————————
    class Fruit(val name:String, val imageId: Int)
    ————————————————————————————————————————————————————————————————————————————

### 2，为子项指定布局
    然后需要为ListView的子项指定一个我们自定义的布局，在layout目录下新建fruit_item.xml，代码如下所示：
    ————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="60dp"> 
     //这里图片的数据找寻是一个难点
     <ImageView 
     android:id="@+id/fruitImage" 
     android:layout_width="40dp" 
     android:layout_height="40dp" 
     android:layout_gravity="center_vertical" 
     android:layout_marginLeft="10dp"/> 
     
     <TextView 
     android:id="@+id/fruitName" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center_vertical" 
     android:layout_marginLeft="10dp" /> 
     
    </LinearLayout> 
    ————————————————————————————————————————————————————————————————————————————

### 3，自定义水果适配器
    接下来需要创建一个自定义的适配器，这个适配器继承自ArrayAdapter，
    并将泛型指定为 Fruit类。新建类FruitAdapter，代码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class FruitAdapter(activity: Activity, val resourceId: Int, data: List<Fruit>) : 
     ArrayAdapter<Fruit>(activity, resourceId, data) { 
     
     override fun getView(position: Int, convertView: View?, parent: ViewGroup): View { 
     val view = LayoutInflater.from(context).inflate(resourceId, parent, false) 
     val fruitImage: ImageView = view.findViewById(R.id.fruitImage) 
     val fruitName: TextView = view.findViewById(R.id.fruitName) 
     val fruit = getItem(position) // 获取当前项的Fruit实例 
     if (fruit != null) { 
     fruitImage.setImageResource(fruit.imageId) 
     fruitName.text = fruit.name 
     } 
     return view 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 4，配置主Activity
    最后修改MainActivity中的代码，如下所示：
    ——————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     private val fruitList = ArrayList<Fruit>() 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     initFruits() // 初始化水果数据 
     val adapter = FruitAdapter(this, R.layout.fruit_item, fruitList) 
     listView.adapter = adapter 
     } 
     
     private fun initFruits() { 
     repeat(2) { 
     fruitList.add(Fruit("Apple", R.drawable.apple_pic)) 
     fruitList.add(Fruit("Banana", R.drawable.banana_pic)) 
     fruitList.add(Fruit("Orange", R.drawable.orange_pic)) 
     fruitList.add(Fruit("Watermelon", R.drawable.watermelon_pic)) 
     fruitList.add(Fruit("Pear", R.drawable.pear_pic)) 
     fruitList.add(Fruit("Grape", R.drawable.grape_pic)) 
     fruitList.add(Fruit("Pineapple", R.drawable.pineapple_pic)) 
     fruitList.add(Fruit("Strawberry", R.drawable.strawberry_pic)) 
     fruitList.add(Fruit("Cherry", R.drawable.cherry_pic)) 
     fruitList.add(Fruit("Mango", R.drawable.mango_pic)) 
     } 
     } 
     
    }
    ——————————————————————————————————————————————————————————————————————————

### 5，（提升ListView的运行效率）
    修改FruitAdapter中的代码，如下所示：
    ————————————————————————————————————————————————————————————————————————————————————
    class FruitAdapter(activity: Activity, val resourceId: Int, data: List<Fruit>) : 
     ArrayAdapter<Fruit>(activity, resourceId, data) { 
     
     inner class ViewHolder(val fruitImage: ImageView, val fruitName: TextView) 
     
     override fun getView(position: Int, convertView: View?, parent: ViewGroup): View { 
     val view: View 
     val viewHolder: ViewHolder 
     if (convertView == null) { 
     view = LayoutInflater.from(context).inflate(resourceId, parent, false) 
     val fruitImage: ImageView = view.findViewById(R.id.fruitImage) 
     val fruitName: TextView = view.findViewById(R.id.fruitName) 
     viewHolder = ViewHolder(fruitImage, fruitName) 
     view.tag = viewHolder 
     } else { 
     view = convertView 
     viewHolder = view.tag as ViewHolder 
     } 
     
     val fruit = getItem(position) // 获取当前项的Fruit实例 
     if (fruit != null) { 
     viewHolder.fruitImage.setImageResource(fruit.imageId) 
     viewHolder.fruitName.text = fruit.name 
     } 
     return view 
     } 
     
    }
    ————————————————————————————————————————————————————————————————————————————————————————

### 6，（添加:ListView的点击事件）
    修改MainActivity中的代码，如下所示：
    ————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     private val fruitList = ArrayList<Fruit>() 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     initFruits() // 初始化水果数据 
     val adapter = FruitAdapter(this, R.layout.fruit_item, fruitList) 
     listView.adapter = adapter 
     listView.setOnItemClickListener { parent, view, position, id -> 
     val fruit = fruitList[position] 
     Toast.makeText(this, fruit.name, Toast.LENGTH_SHORT).show() 
     } 
     } 
     
     ... 
     
    } 
    ————————————————————————————————————————————————————————————————————————————

## 九，RecyclerView的基本用法

### 1，
    打开app/build.gradle文件，在dependencies闭包中添加如下内容：
    ——————————————————————————————————————————————————————————— 
    implementation 'androidx.recyclerview:recyclerview:1.0.0' 
    ———————————————————————————————————————————————————————————

### 2，
    接下来修改activity_main.xml中的代码，如下所示：
    ———————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <androidx.recyclerview.widget.RecyclerView 
     android:id="@+id/recyclerView" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" /> 
     
    </LinearLayout> 
    ———————————————————————————————————————————————————————————

### 3，
    接下来需要为RecyclerView准备一个适配器，新建FruitAdapter类，让这个适配器继承自
    RecyclerView.Adapter，并将泛型指定为FruitAdapter.ViewHolder。其中，
    ViewHolder是我们在FruitAdapter中定义的一个内部类，代码如下所示：
    ———————————————————————————————————————————————————————————
    class FruitAdapter(val fruitList: List<Fruit>) : 
     RecyclerView.Adapter<FruitAdapter.ViewHolder>() { 
     
     inner class ViewHolder(view: View) : RecyclerView.ViewHolder(view) { 
     val fruitImage: ImageView = view.findViewById(R.id.fruitImage) 
     val fruitName: TextView = view.findViewById(R.id.fruitName) 
     } 
     
     override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder { 
     val view = LayoutInflater.from(parent.context) 
     .inflate(R.layout.fruit_item, parent, false) 
     return ViewHolder(view) 
     } 
     
     override fun onBindViewHolder(holder: ViewHolder, position: Int) { 
     val fruit = fruitList[position] 
     holder.fruitImage.setImageResource(fruit.imageId) 
     holder.fruitName.text = fruit.name 
     } 
     
     override fun getItemCount() = fruitList.size 
     
    } 
    ———————————————————————————————————————————————————————————

### 4，
    修改MainActivity中的代码
    ———————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     private val fruitList = ArrayList<Fruit>() 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     initFruits() // 初始化水果数据 
     val layoutManager = LinearLayoutManager(this) 
     recyclerView.layoutManager = layoutManager 
     val adapter = FruitAdapter(fruitList) 
     recyclerView.adapter = adapter 
     } 
     
     private fun initFruits() { 
     repeat(2) { 
     fruitList.add(Fruit("Apple", R.drawable.apple_pic)) 
     fruitList.add(Fruit("Banana", R.drawable.banana_pic)) 
     fruitList.add(Fruit("Orange", R.drawable.orange_pic)) 
     fruitList.add(Fruit("Watermelon", R.drawable.watermelon_pic)) 
     fruitList.add(Fruit("Pear", R.drawable.pear_pic)) 
     fruitList.add(Fruit("Grape", R.drawable.grape_pic)) 
     fruitList.add(Fruit("Pineapple", R.drawable.pineapple_pic)) 
     fruitList.add(Fruit("Strawberry", R.drawable.strawberry_pic)) 
     fruitList.add(Fruit("Cherry", R.drawable.cherry_pic)) 
     fruitList.add(Fruit("Mango", R.drawable.mango_pic)) 
     } 
     } 
     
    } 
    ———————————————————————————————————————————————————————————

## 十，实现横向滚动和瀑布流布局

### 1，
    在上一节的基础上修改fruit_item.xml中的代码，如下所示：
    ———————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="80dp" 
     android:layout_height="wrap_content"> 
     
     <ImageView 
     android:id="@+id/fruitImage" 
     android:layout_width="40dp" 
     android:layout_height="40dp" 
     android:layout_gravity="center_horizontal" 
     android:layout_marginTop="10dp" /> 
     
     <TextView 
     android:id="@+id/fruitName" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center_horizontal" 
     android:layout_marginTop="10dp" /> 
     
    </LinearLayout>
    ———————————————————————————————————————————————————————————

### 2，
    接下来修改MainActivity中的代码，如下所示：
    ———————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     private val fruitList = ArrayList<Fruit>() 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     initFruits() // 初始化水果数据 
     val layoutManager = LinearLayoutManager(this) 
     layoutManager.orientation = LinearLayoutManager.HORIZONTAL 
     recyclerView.layoutManager = layoutManager 
     val adapter = FruitAdapter(fruitList) 
     recyclerView.adapter = adapter 
     }  
     ... 
    }
    ———————————————————————————————————————————————————————————

### 3，
    里我们来实现一下效果更加炫酷的瀑布流布局
    首先还是来修改一下fruit_item.xml中的代码，如下所示：
    ———————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:layout_margin="5dp"> 
     
     <ImageView 
     android:id="@+id/fruitImage" 
     android:layout_width="40dp" 
     android:layout_height="40dp" 
     android:layout_gravity="center_horizontal" 
     android:layout_marginTop="10dp" /> 
     
     <TextView 
     android:id="@+id/fruitName" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="left" 
     android:layout_marginTop="10dp" /> 
     
    </LinearLayout>
    ———————————————————————————————————————————————————————————

### 4，
    接着修改MainActivity中的代码，如下所示：
     ———————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     private val fruitList = ArrayList<Fruit>() 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     initFruits() // 初始化水果数据 
     val layoutManager = StaggeredGridLayoutManager(3, 
     StaggeredGridLayoutManager.VERTICAL) 
     recyclerView.layoutManager = layoutManager 
     val adapter = FruitAdapter(fruitList) 
     recyclerView.adapter = adapter 
     } 
     
     private fun initFruits() { 
     repeat(2) {  
     fruitList.add(Fruit(getRandomLengthString("Apple"), 
     R.drawable.apple_pic)) 
     fruitList.add(Fruit(getRandomLengthString("Banana"), 
     R.drawable.banana_pic)) 
     fruitList.add(Fruit(getRandomLengthString("Orange"), 
     R.drawable.orange_pic)) 
     fruitList.add(Fruit(getRandomLengthString("Watermelon"), 
     R.drawable.watermelon_pic)) 
     fruitList.add(Fruit(getRandomLengthString("Pear"), 
     R.drawable.pear_pic)) 
     fruitList.add(Fruit(getRandomLengthString("Grape"), 
     R.drawable.grape_pic)) 
     fruitList.add(Fruit(getRandomLengthString("Pineapple"), 
     R.drawable.pineapple_pic)) 
     fruitList.add(Fruit(getRandomLengthString("Strawberry"), 
     R.drawable.strawberry_pic)) 
     fruitList.add(Fruit(getRandomLengthString("Cherry"), 
     R.drawable.cherry_pic)) 
     fruitList.add(Fruit(getRandomLengthString("Mango"), 
     R.drawable.mango_pic)) 
     } 
     } 
     
     private fun getRandomLengthString(str: String): String { 
     val n = (1..20).random() 
     val builder = StringBuilder() 
     repeat(n) { 
     builder.append(str) 
     } 
     return builder.toString() 
     } 
     
    } 
     ———————————————————————————————————————————————————————————

## 十一，编辑RecyclerView的点击事件
    上面创建好ResyclerView的基础上
### 1,
    修改FruitAdapter中的代码，如下所示：
    ———————————————————————————————————————————————————————————
    class FruitAdapter(val fruitList: List<Fruit>) : 
     RecyclerView.Adapter<FruitAdapter.ViewHolder>() { 
     ... 
     override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder { 
     val view = LayoutInflater.from(parent.context) 
     .inflate(R.layout.fruit_item, parent, false) 
     val viewHolder = ViewHolder(view) 
     viewHolder.itemView.setOnClickListener { 
     val position = viewHolder.adapterPosition 
     val fruit = fruitList[position] 
     Toast.makeText(parent.context, "you clicked view ${fruit.name}", 
     Toast.LENGTH_SHORT).show() 
     } 
     viewHolder.fruitImage.setOnClickListener { 
     val position = viewHolder.adapterPosition 
     val fruit = fruitList[position] 
     Toast.makeText(parent.context, "you clicked image ${fruit.name}", 
     Toast.LENGTH_SHORT).show() 
     } 
     return viewHolder 
     } 
     ... 
    }
    ———————————————————————————————————————————————————————————

## 十三，编写精美的聊天界面

### 1，制作9.Patch图片
    先在UIBestPractice项目中放置一张气泡样式的图片message_left.png
    我们将这张图片设置为LinearLayout的背景图片，修改activity_main.xml中的代码
    ———————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="50dp" 
     android:background="@drawable/message_left"> 
    </LinearLayout> 
    ———————————————————————————————————————————————————————————
    但效果不好
    此处省略详细的9.Patch图片的制作过程
    message_left.9.png可以作为收到消息的背景图，那么毫无疑问你还需要再制作一张
    message_right.9.png作为发出消息的背景图。制作过程是完全一样的
    开始项目之前还需要添加依赖库
    implementation 'androidx.recyclerview:recyclerview:1.0.0'

### 2，编写主布局
    接下来开始编写主界面，修改activity_main.xml中的代码，如下所示：
    ———————————————————————————————————————————————————————————
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#d8e0e8" >
    
        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1" />
    
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content" >
    
            <EditText
                android:id="@+id/inputText"
                android:layout_width="0dp"
                android:layout_height="48dp"
                android:layout_weight="1"
                android:hint="@string/hint_input"
                android:maxLines="2"
                android:inputType="textMultiLine"
                android:minHeight="48dp"
                android:layout_marginBottom="80dp"/>
    
            <Button
                android:id="@+id/send"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Send"
                android:layout_marginBottom="80dp"/>
    
        </LinearLayout>
    
    </LinearLayout>
    ———————————————————————————————————————————————————————————

### 3，新建消息实体类
    然后定义消息的实体类，新建Msg，代码如下所示：
    ———————————————————————————————————————————————————————————
    class Msg(val content: String, val type: Int) { 
     companion object { 
     const val TYPE_RECEIVED = 0 
     const val TYPE_SENT = 1 
     } 
    } 
    ———————————————————————————————————————————————————————————

### 4， 左边的对话框布局
    接下来开始编写RecyclerView的子项布局，新建msg_left_item.xml，代码如下所示：
    ———————————————————————————————————————————————————————————
        <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="10dp" >
    
        <LinearLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="left"
            android:background="@drawable/message_left"
            android:layout_marginTop="80dp">
    
            <TextView
                android:id="@+id/leftMsg"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:layout_margin="10dp"
                android:textColor="#fff"
                />
    
        </LinearLayout>
    
    </FrameLayout>
    ———————————————————————————————————————————————————————————

### 5，右边的对话框布局
    类似地，我们还需要再编写一个发送消息的子项布局，新建msg_right_item.xml，代码如下所示：
    ———————————————————————————————————————————————————————————
    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:padding="10dp" > 
     
     <LinearLayout 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="right" 
     android:background="@drawable/message_right" > 
     
     <TextView 
     android:id="@+id/rightMsg" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center" 
     android:layout_margin="10dp" 
     android:textColor="#000" /> 
     
     </LinearLayout> 
     
    </FrameLayout>
    ———————————————————————————————————————————————————————————

### 6，配置适配器
    接下来需要创建RecyclerView的适配器类，新建类MsgAdapter，代码如下所示：
    ———————————————————————————————————————————————————————————
    class MsgAdapter(val msgList: List<Msg>) : RecyclerView.Adapter<RecyclerView.ViewHolder>() { 
     
     inner class LeftViewHolder(view: View) : RecyclerView.ViewHolder(view) { 
     val leftMsg: TextView = view.findViewById(R.id.leftMsg) 
     } 
     
     inner class RightViewHolder(view: View) : RecyclerView.ViewHolder(view) { 
     val rightMsg: TextView = view.findViewById(R.id.rightMsg) 
     } 
     
     override fun getItemViewType(position: Int): Int { 
     val msg = msgList[position] 
     return msg.type 
     } 
     
     override fun onCreateViewHolder(parent: ViewGroup, viewType: Int) = if (viewType == 
     Msg.TYPE_RECEIVED) { 
     val view = LayoutInflater.from(parent.context).inflate(R.layout.msg_left_item, 
     parent, false) 
     LeftViewHolder(view) 
     } else { 
     val view = LayoutInflater.from(parent.context).inflate(R.layout.msg_right_item, 
     parent, false) 
     RightViewHolder(view) 
     } 
     
     override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) { 
     val msg = msgList[position] 
     when (holder) { 
     is LeftViewHolder -> holder.leftMsg.text = msg.content 
     is RightViewHolder -> holder.rightMsg.text = msg.content 
     else -> throw IllegalArgumentException() 
     } 
     } 
     
     override fun getItemCount() = msgList.size 
     
    } 
    ———————————————————————————————————————————————————————————

### 7，编写主Activity
    最后修改MainActivity中的代码，记得完成注册，
    为RecyclerView初始化一些数据，并给发送按钮加入事件响应，代码如下所示：
    ———————————————————————————————————————————————————————————
    import android.os.Bundle
    import android.view.View
    import android.widget.Button
    import android.widget.EditText
    import androidx.appcompat.app.AppCompatActivity
    import androidx.recyclerview.widget.LinearLayoutManager
    import androidx.recyclerview.widget.RecyclerView
    
    class MainActivity : AppCompatActivity(), View.OnClickListener {
    
        private val msgList = ArrayList<Msg>()
        private var adapter: MsgAdapter? = null
    
        // 添加 lateinit 字段
        private lateinit var recyclerView: RecyclerView
        private lateinit var send: Button // 这里的 'send' 现在指向 R.id.send
        private lateinit var inputText: EditText
    
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
    
            // 绑定视图
            recyclerView = findViewById(R.id.recyclerView)
            send = findViewById(R.id.send)
            inputText = findViewById(R.id.inputText)
    
            initMsg()
    
            val layoutManager = LinearLayoutManager(this)
            recyclerView.layoutManager = layoutManager
            adapter = MsgAdapter(msgList)
            recyclerView.adapter = adapter
            send.setOnClickListener(this)
        }
    
        override fun onClick(v: View?) {
            when (v) {
                send -> {
                    val content = inputText.text.toString()
                    if (content.isNotEmpty()) {
                        val msg = Msg(content, Msg.TYPE_SENT)
                        msgList.add(msg)
                        adapter?.notifyItemInserted(msgList.size - 1)
                        recyclerView.scrollToPosition(msgList.size - 1)
                        inputText.setText("")
                    }
                }
            }
        }
    
        private fun initMsg() {
            val msg1 = Msg("Hello guy.", Msg.TYPE_RECEIVED)
            msgList.add(msg1)
            val msg2 = Msg("Hello. Who is that?", Msg.TYPE_SENT)
            msgList.add(msg2)
            val msg3 = Msg("This is Tom. Nice talking to you. ", Msg.TYPE_RECEIVED)
            msgList.add(msg3)
        }
    }
    ———————————————————————————————————————————————————————————

# 《三》探究Fragment

## 一，初步使用Fragment

### 1，创建布局
    先尝试实现在一个Activity当中添加两个Fragment，并让这两个Fragment平分Activity的空间。
    新建一个左侧Fragment的布局left_fragment.xml，代码如下所示：
    ——————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <Button 
     android:id="@+id/button" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center_horizontal" 
     android:text="Button" 
     />
    </LinearLayout> 
    ——————————————————————————————————————————————————————————————————————————

    然后新建右侧Fragment的布局right_fragment.xml，代码如下所示：
    ——————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:background="#00ff00" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <TextView 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center_horizontal" 
     android:textSize="24sp" 
     android:text="This is right fragment" 
     /> 
     
    </LinearLayout>
    ——————————————————————————————————————————————————————————————————————————

### 2，加载布局
    接着新建一个LeftFragment类，并让它继承自Fragment。注意，这里可能会有两个不同包
    下的Fragment供你选择：一个是系统内置的android.app.Fragment，一个是AndroidX库中
    的androidx.fragment.app.Fragment。这里请一定要使用AndroidX库中的Fragment。
    ——————————————————————————————————————————————————————————————————————————
    import android.os.Bundle
    import android.view.LayoutInflater
    import android.view.View
    import android.view.ViewGroup
    import android.widget.Button
    import androidx.fragment.app.Fragment
    
    class LeftFragment : Fragment() {
        override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? {
            val view = inflater.inflate(R.layout.left_fragment, container, false)
            view.findViewById<Button>(R.id.button).setOnClickListener {
                // 确保 replaceFragment 是 public
                (activity as MainActivity).replaceFragment(AnotherRightFragment())
            }
            return view
        }
    }
    ——————————————————————————————————————————————————————————————————————————
    用来加载上面创建的布局
    
    接着我们用同样的方法再新建一个RightFragment
    ——————————————————————————————————————————————————————————————————————————
    package com.example.fifth1
    
    import android.os.Bundle
    import android.view.LayoutInflater
    import android.view.View
    import android.view.ViewGroup
    import androidx.fragment.app.Fragment
    
    class RightFragment : Fragment() {
        override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? {
            return inflater.inflate(R.layout.right_fragment, container, false)
        }
    }
    ——————————————————————————————————————————————————————————————————————————

### 3，设置主activity布局
    接下来修改activity_main.xml中的代码，通过android:name属性来显式声明要添加的Fragment类名，
    注意一定要将类的包名也加上。如下所示：
    ——————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="horizontal" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <fragment 
     android:id="@+id/leftFrag" 
     android:name="com.example.fragmenttest.LeftFragment" 
     android:layout_width="0dp" 
     android:layout_height="match_parent" 
     android:layout_weight="1" /> 
     
     <fragment 
     android:id="@+id/rightFrag" 
     android:name="com.example.fragmenttest.RightFragment" 
     android:layout_width="0dp" 
     android:layout_height="match_parent" 
     android:layout_weight="1" /> 
     
    </LinearLayout> 
    ——————————————————————————————————————————————————————————————————————————
    至此，两个Fragment平分了整个Activity的布局。

### 4，进阶再设一个fragment布局
    继续新建another_right_fragment.xml，代码如下所示：
    ——————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:background="#ffff00" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <TextView 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center_horizontal" 
     android:textSize="24sp" 
     android:text="This is another right fragment" 
     /> 
     
    </LinearLayout>
    ——————————————————————————————————————————————————————————————————————————

### 5，同理加载布局
    然后新建AnotherRightFragment作为另一个右侧Fragment，代码如下所示：
    ——————————————————————————————————————————————————————————————————————————
    package com.example.fifth1
    
    import android.os.Bundle
    import android.view.LayoutInflater
    import android.view.View
    import android.view.ViewGroup
    import androidx.fragment.app.Fragment
    
    class AnotherRightFragment : Fragment() {
        override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? {
            return inflater.inflate(R.layout.another_right_fragment, container, false)
        }
    }
    ——————————————————————————————————————————————————————————————————————————

### 6，修改主布局
    接下来看一下将它动态地添加到Activity当中。修改activity_main.xml，代码如下所示：
    ——————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="horizontal" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <fragment 
     android:id="@+id/leftFrag" 
     android:name="com.example.fragmenttest.LeftFragment" 
     android:layout_width="0dp" 
     android:layout_height="match_parent" 
     android:layout_weight="1" /> 
     
     <FrameLayout //关键变化
     android:id="@+id/rightLayout" 
     android:layout_width="0dp" 
     android:layout_height="match_parent" 
     android:layout_weight="1" > 
     </FrameLayout> 
     
    </LinearLayout> 
    ——————————————————————————————————————————————————————————————————————————

### 7，添加replaceFragment（）
    下面我们将在代码中向FrameLayout里添加内容，从而实现动态添加Fragment的功能。
    修改MainActivity中的代码，如下所示：
    ——————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            replaceFragment(RightFragment())
        }
     
     private fun replaceFragment(fragment: Fragment) { 
     val fragmentManager = supportFragmentManager 
     val transaction = fragmentManager.beginTransaction() 
     transaction.replace(R.id.rightLayout, fragment) 
     transaction.commit() 
     } 
     
    }
    ——————————————————————————————————————————————————————————————————————————
    这样就成功实现了向Activity中动态添加Fragment的功能。

### 8，fragment实现返回栈
    想要实现类似于返回栈的效果，按下Back键可以回到上一个Fragment
    修改MainActivity中的代码，如下所示：
    ——————————————————————————————————————————————————————————————————————————
    import android.os.Bundle
    import androidx.appcompat.app.AppCompatActivity
    import androidx.fragment.app.Fragment
    
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            replaceFragment(RightFragment())
        }
    
        fun replaceFragment(fragment: Fragment) {
            supportFragmentManager.beginTransaction()
                .replace(R.id.rightLayout, fragment)
                .addToBackStack(null)
                .commit()
        }
    }
    ——————————————————————————————————————————————————————————————————————————
    如果发现运行不了，那你就可以查看logcat日志，现在的错误就是可以在AndroidMainfest的文件里面设置android:theme
    android:theme="@style/Theme.AppCompat.Light.DarkActionBar">

### 9，（实现Fragment和Activity之间的交互）
       为了方便Fragment和Activity之间进行交互，FragmentManager提供了一个类似于
    findViewById()的方法，专门用于从布局文件中获取Fragment的实例，代码如下所示：
    val fragment = supportFragmentManager.findFragmentById(R.id.leftFrag) as LeftFragment 
    调用FragmentManager的findFragmentById()方法，可以在Activity中得到相应
    Fragment的实例，然后就能轻松地调用Fragment里的方法了。
    另外，类似于findViewById()方法，kotlin-android-extensions插件也对
    findFragmentById()方法进行了扩展，允许我们直接使用布局文件中定义的Fragment id名
    称来自动获取相应的Fragment实例，如下所示：
    val fragment = leftFrag as LeftFragment 
    那么毫无疑问，第二种写法是我们现在更加推荐的写法。
    掌握了如何在Activity中调用Fragment里的方法，那么在Fragment中又该怎样调用Activity
    里的方法呢？这就更简单了，在每个Fragment中都可以通过调用getActivity()方法来得到
    和当前Fragment相关联的Activity实例，代码如下所示：
    if (activity != null) { 
     val mainActivity = activity as MainActivity 
    } 
    这里由于getActivity()方法有可能返回null，因此我们需要先进行一个判空处理。有了
    Activity的实例，在Fragment中调用Activity里的方法就变得轻而易举了。另外当Fragment
    中需要使用Context对象时，也可以使用getActivity()方法，因为获取到的Activity本身就
    是一个Context对象。

### 10，（体验Fragment的生命周期）
    类似Activity的生命周期？暂时还没有能记住，以后需要使用再做回顾吧
    在上一节的基础上，修改RightFragment中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class RightFragment : Fragment() { 
     
     companion object { 
     const val TAG = "RightFragment" 
     } 
     
     override fun onAttach(context: Context) { 
     super.onAttach(context) 
     Log.d(TAG, "onAttach") 
     } 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     Log.d(TAG, "onCreate") 
     } 
     
     override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, 
     savedInstanceState: Bundle?): View? { 
     Log.d(TAG, "onCreateView") 
     return inflater.inflate(R.layout.right_fragment, container, false) 
     } 
     
     override fun onActivityCreated(savedInstanceState: Bundle?) { 
     super.onActivityCreated(savedInstanceState) 
     Log.d(TAG, "onActivityCreated") 
     } 
     
     override fun onStart() { 
     super.onStart() 
     Log.d(TAG, "onStart") 
     } 
     
     override fun onResume() { 
     super.onResume() 
     Log.d(TAG, "onResume") 
     } 
     
     override fun onPause() { 
     super.onPause() 
     Log.d(TAG, "onPause") 
     } 
     
     override fun onStop() { 
     super.onStop() 
     Log.d(TAG, "onStop") 
     } 
     
     override fun onDestroyView() { 
     super.onDestroyView() 
     Log.d(TAG, "onDestroyView") 
     } 
     
     override fun onDestroy() { 
     super.onDestroy() 
     Log.d(TAG, "onDestroy") 
     } 
     
     override fun onDetach() { 
     super.onDetach() 
     Log.d(TAG, "onDetach") 
     } 
     
    } 
    —————————————————————————————————————————————————————————————————————————————

## 二，动态选择加载布局

### 1，主布局只保留一个fragment
    想要实现不同的设备切换加载单页模式还是双页模式，
    修改FragmentTest项目中的activity_main.xml文件，代码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="horizontal" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <fragment 
     android:id="@+id/leftFrag" 
     android:name="com.example.fragmenttest.LeftFragment" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"/> 
     
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————
    如果是在上一节的基础上来继续写的，记得要删掉多余的代码，不然会报错

### 2，设置大屏幕的布局
    接着在res目录下新建layout-large文件夹，在这个文件夹下新建一个布局，也叫作activity_main.xml
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="horizontal" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <fragment 
     android:id="@+id/leftFrag" 
     android:name="com.example.fragmenttest.LeftFragment" 
     android:layout_width="0dp" 
     android:layout_height="match_parent" 
     android:layout_weight="1" /> 
     
     <fragment 
     android:id="@+id/rightFrag" 
     android:name="com.example.fragmenttest.RightFragment" 
     android:layout_width="0dp" 
     android:layout_height="match_parent" 
     android:layout_weight="3" /> 
     
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————
    然后将MainActivity中replaceFragment()方法里的代码注释掉，并在平板模拟器上重新运行程序
    就实现了在手机和平板上加载不同界面的效果
    这里的原理是large是一个限定符， 当被认定是大屏时就会选择加载大的

### 3，最小宽度限制
    如果想要使用最小宽度限定符，
    在res目录下新建layout-sw600dp文件夹，然后在这个文件夹下新建activity_main.xml布局，
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="horizontal" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <fragment 
     android:id="@+id/leftFrag" 
     android:name="com.example.fragmenttest.LeftFragment" 
     android:layout_width="0dp" 
     android:layout_height="match_parent" 
     android:layout_weight="1" /> 
     
     <fragment 
     android:id="@+id/rightFrag" 
     android:name="com.example.fragmenttest.RightFragment" 
     android:layout_width="0dp" 
     android:layout_height="match_parent" 
     android:layout_weight="3" /> 
     
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

## 三，制作一个一个简易版的新闻应用

### 1. 创建项目
    打开Android Studio → New Project → Empty Activity
    Name: FragmentBestPractice
    Language: Kotlin
    Minimum SDK: API 21: Android 5.0 (Lollipop)（可选）

### 2. 添加RecyclerView依赖
    打开 app/build.gradle，在 dependencies 中添加：
    —————————————————————————————————————————————————————————————————————————————
    implementation 'androidx.recyclerview:recyclerview:1.0.0'
    —————————————————————————————————————————————————————————————————————————————

### 3. 创建News类
    右键点击 com.example.fragmentbestpractice 包 → New → Kotlin Class/File  Name: News
    —————————————————————————————————————————————————————————————————————————————
    class News(val title: String, val content: String)
    —————————————————————————————————————————————————————————————————————————————

### 4. 创建布局文件 res/layout/news_content_frag.xml
    —————————————————————————————————————————————————————————————————————————————
       <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
           android:layout_width="match_parent"
           android:layout_height="match_parent">
    
           <LinearLayout
               android:id="@+id/contentLayout"
               android:layout_width="match_parent"
               android:layout_height="match_parent"
               android:orientation="vertical"
               android:visibility="invisible">
    
               <TextView
                   android:id="@+id/newsTitle"
                   android:layout_width="match_parent"
                   android:layout_height="wrap_content"
                   android:gravity="center"
                   android:padding="10dp"
                   android:textSize="20sp" />
    
               <View
                   android:layout_width="match_parent"
                   android:layout_height="1dp"
                   android:background="#000" />
    
               <TextView
                   android:id="@+id/newsContent"
                   android:layout_width="match_parent"
                   android:layout_height="0dp"
                   android:layout_weight="1"
                   android:padding="15dp"
                   android:textSize="18sp" />
    
           </LinearLayout>
    
           <View
               android:layout_width="1dp"
               android:layout_height="match_parent"
               android:layout_alignParentLeft="true"
               android:background="#000" />
    
    </RelativeLayout>
    —————————————————————————————————————————————————————————————————————————————

### 5. 创建 NewsContentFragment.kt
    右键点击包 → New → Kotlin Class/File → Name: NewsContentFragment
    —————————————————————————————————————————————————————————————————————————————
    import android.os.Bundle
    import android.view.LayoutInflater
    import android.view.View
    import android.view.ViewGroup
    import android.widget.TextView
    import androidx.fragment.app.Fragment
    
    class NewsContentFragment : Fragment() {
        private lateinit var contentLayout: View
        private lateinit var newsTitle: TextView
        private lateinit var newsContent: TextView
    
        override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? {
            //加载刚刚创建的布局
            return inflater.inflate(R.layout.news_content_frag, container, false)
        }
    
        override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)
            contentLayout = view.findViewById(R.id.contentLayout)
            newsTitle = view.findViewById(R.id.newsTitle)
            newsContent = view.findViewById(R.id.newsContent)
        }
    
        fun refresh(title: String, content: String) {
            contentLayout.visibility = View.VISIBLE
            newsTitle.text = title
            newsContent.text = content
        }
    }
    —————————————————————————————————————————————————————————————————————————————

### 6. 创建 NewsContentActivity.kt
    右键点击包 → New → Activity → Empty Activity
    —————————————————————————————————————————————————————————————————————————————
    import android.content.Context
    import android.content.Intent
    import android.os.Bundle
    import androidx.appcompat.app.AppCompatActivity
    
    class NewsContentActivity : AppCompatActivity() {
        companion object {
            fun actionStart(context: Context, title: String, content: String) {
                val intent = Intent(context, NewsContentActivity::class.java).apply {
                    putExtra("news_title", title)
                    putExtra("news_content", content)
                }
                context.startActivity(intent)
            }
        }
    
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_news_content)
    
            val title = intent.getStringExtra("news_title")
            val content = intent.getStringExtra("news_content")
            if (title != null && content != null) {
                val fragment = supportFragmentManager.findFragmentById(R.id.newsContentFrag) as NewsContentFragment
                fragment.refresh(title, content)
            }
        }
    }
    —————————————————————————————————————————————————————————————————————————————

### 7. 创建 res/layout/activity_news_content.xml
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    
        <fragment
            android:id="@+id/newsContentFrag"
            android:name="com.example.fragmentbestpractice.NewsContentFragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            />
    
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 8. 创建 res/layout/news_title_frag.xml
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    
        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/newsTitleRecyclerView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            />
    
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 9. 创建 res/layout/news_item.xml
    —————————————————————————————————————————————————————————————————————————————
    <TextView xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/newsTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:maxLines="1"
        android:ellipsize="end"
        android:textSize="18sp"
        android:paddingLeft="10dp"
        android:paddingRight="10dp"
        android:paddingTop="15dp"
        android:paddingBottom="15dp" />
    —————————————————————————————————————————————————————————————————————————————

### 10. 创建 NewsTitleFragment.kt
    右键点击包 → New → Kotlin Class/File → Name: NewsTitleFragment
    —————————————————————————————————————————————————————————————————————————————
    import android.os.Bundle
    import android.view.LayoutInflater
    import android.view.View
    import android.view.ViewGroup
    import androidx.fragment.app.Fragment
    import androidx.recyclerview.widget.LinearLayoutManager
    import androidx.recyclerview.widget.RecyclerView
    
    class NewsTitleFragment : Fragment() {
        private var isTwoPane = false
    
        override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? {
            return inflater.inflate(R.layout.news_title_frag, container, false)
        }
    
        override fun onActivityCreated(savedInstanceState: Bundle?) {
            super.onActivityCreated(savedInstanceState)
            isTwoPane = activity?.findViewById<View>(R.id.newsContentLayout) != null
            val recyclerView = requireView().findViewById<RecyclerView>(R.id.newsTitleRecyclerView)
            recyclerView.layoutManager = LinearLayoutManager(activity)
            recyclerView.adapter = NewsAdapter(getNews())
        }
    
        inner class NewsAdapter(val newsList: List<News>) : RecyclerView.Adapter<NewsAdapter.ViewHolder>() {
            inner class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
                val newsTitle: TextView = view.findViewById(R.id.newsTitle)
            }
    
            override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
                val view = LayoutInflater.from(parent.context).inflate(R.layout.news_item, parent, false)
                val holder = ViewHolder(view)
                holder.itemView.setOnClickListener {
                    val news = newsList[holder.adapterPosition]
                    if (isTwoPane) {
                        val fragment = activity?.supportFragmentManager?.findFragmentById(R.id.newsContentFrag) as NewsContentFragment
                        fragment.refresh(news.title, news.content)
                    } else {
                        NewsContentActivity.actionStart(requireContext(), news.title, news.content)
                    }
                }
                return holder
            }
    
            override fun onBindViewHolder(holder: ViewHolder, position: Int) {
                holder.newsTitle.text = newsList[position].title
            }
    
            override fun getItemCount() = newsList.size
        }
    
        private fun getNews(): List<News> {
            val newsList = ArrayList<News>()
            for (i in 1..50) {
                val news = News("This is news title $i", getRandomLengthString("This is news content $i. "))
                newsList.add(news)
            }
            return newsList
        }
    
        private fun getRandomLengthString(str: String): String {
            val n = (1..20).random()
            return StringBuilder().apply { repeat(n) { append(str) } }.toString()
        }
    }
    —————————————————————————————————————————————————————————————————————————————

### 11. 创建 MainActivity.kt
    右键点击包 → New → Activity → Empty Activity
    —————————————————————————————————————————————————————————————————————————————
    import android.os.Bundle
    import androidx.appcompat.app.AppCompatActivity
    
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
        }
    }
    —————————————————————————————————————————————————————————————————————————————

### 12. 创建布局文件 res/layout/activity_main.xml
    —————————————————————————————————————————————————————————————————————————————
    <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/newsTitleLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    
        <fragment
            android:id="@+id/newsTitleFrag"
            android:name="com.example.fragmentbestpractice.NewsTitleFragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            />
    
    </FrameLayout>
    —————————————————————————————————————————————————————————————————————————————

### 13. 创建双屏布局 res/layout-sw600dp/activity_main.xml
    右键点击 res 文件夹 → New → Android Resource Directory
    Resource type: layout
    Resource qualifier: sw600dp (smallest width 600dp)
    在新创建的 layout-sw600dp 文件夹中创建 activity_main.xml
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    
        <fragment
            android:id="@+id/newsTitleFrag"
            android:name="com.example.fragmentbestpractice.NewsTitleFragment"
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1" />
    
        <FrameLayout
            android:id="@+id/newsContentLayout"
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="3">
    
            <fragment
                android:id="@+id/newsContentFrag"
                android:name="com.example.fragmentbestpractice.NewsContentFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
        </FrameLayout>
    
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 14. 修改 AndroidManifest.xml
    —————————————————————————————————————————————————————————————————————————————
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.example.fragmentbestpractice">
    
        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/AppTheme">
            <activity android:name=".MainActivity">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
            <activity android:name=".NewsContentActivity" />
        </application>
    
    </manifest>
    —————————————————————————————————————————————————————————————————————————————

# 《四》详解广播机制

## 一，动态注册监听时间变化

### 动态注册
    新建一个BroadcastTest项目，然后修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     lateinit var timeChangeReceiver: TimeChangeReceiver 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val intentFilter = IntentFilter() 
     intentFilter.addAction("android.intent.action.TIME_TICK") //指定想要接的广播
     timeChangeReceiver = TimeChangeReceiver() 
     registerReceiver(timeChangeReceiver, intentFilter) //传入两个实例
        } 
      
     override fun onDestroy() { 
     super.onDestroy() 
     unregisterReceiver(timeChangeReceiver) //动态注册要取消注册
     } 
     
     inner class TimeChangeReceiver : BroadcastReceiver() { 
     
     override fun onReceive(context: Context, intent: Intent) { 
     Toast.makeText(context, "Time has changed", Toast.LENGTH_SHORT).show() //显示时间变化
     } 
     
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

## 二，静态注册实现开机启动
    模拟器运行时竟然失败了，ohno
### 1，重写显示化
    上一小节中我们是使用内部类的方式创建的BroadcastReceiver，其实还可以通过Android Studio
    提供的快捷方式来创建。右击com.example.broadcasttest包→New→Other→Broadcast Receiver
    这里我们将创建的类命名为BootCompleteReceiver，Exported属性表示是否允
    许这个BroadcastReceiver接收本程序以外的广播，Enabled属性表示是否启用这个
    BroadcastReceiver。勾选这两个属性，点击“Finish”完成创建。
    —————————————————————————————————————————————————————————————————————————————
    class BootCompleteReceiver : BroadcastReceiver() { 
     
     override fun onReceive(context: Context, intent: Intent) { 
     Toast.makeText(context, "Boot Complete", Toast.LENGTH_LONG).show() //简单的Toast
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 2，静态注册
    对AndroidManifest.xml文件进行修改
    —————————————————————————————————————————————————————————————————————————————
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" 
     package="com.example.broadcasttest"> 
     
     <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" /> 
     
     <application 
     android:allowBackup="true" 
     android:icon="@mipmap/ic_launcher" 
     android:label="@string/app_name" 
     android:roundIcon="@mipmap/ic_launcher_round" 
     android:supportsRtl="true" 
     android:theme="@style/AppTheme"> 
     ... 
     <receiver 
     android:name=".BootCompleteReceiver" 
     android:enabled="true" 
     android:exported="true"> 
     <intent-filter> 
     <action android:name="android.intent.action.BOOT_COMPLETED" /> 
     </intent-filter> 
     </receiver> 
     </application> 
     
    </manifest>
    —————————————————————————————————————————————————————————————————————————————

## 三，发送标准广播

### 1，创建Receiver
    新建一个MyBroadcastReceiver，并在onReceive()方法中加入如下代码：
    —————————————————————————————————————————————————————————————————————————————
    class MyBroadcastReceiver : BroadcastReceiver() { 
     
     override fun onReceive(context: Context, intent: Intent) { 
     Toast.makeText(context, "received in MyBroadcastReceiver", //简单的发送Toast
     Toast.LENGTH_SHORT).show() 
     } 
     
    } 
    —————————————————————————————————————————————————————————————————————————————

### 2，注册声明广播的内容
    然后在AndroidManifest.xml中对这个BroadcastReceiver进行修改,声明广播的内容：
    —————————————————————————————————————————————————————————————————————————————
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" 
     package="com.example.broadcasttest"> 
     ... 
     <application 
     android:allowBackup="true" 
     android:icon="@mipmap/ic_launcher" 
     android:label="@string/app_name" 
     android:roundIcon="@mipmap/ic_launcher_round" 
     android:supportsRtl="true" 
     android:theme="@style/AppTheme"> 
     ... 
     <receiver 
     android:name=".MyBroadcastReceiver" 
     android:enabled="true" 
     android:exported="true"> 
     <intent-filter> 
     <action android:name="com.example.broadcasttest.MY_BROADCAST"/> 
     </intent-filter> 
     </receiver> 
     </application> 
    </manifest>
    —————————————————————————————————————————————————————————————————————————————

### 3，编辑点击布局
    接下来修改activity_main.xml中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <Button 
     android:id="@+id/button" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Send Broadcast" 
     /> 
     
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————

### 4，编辑点击发送事件
    然后修改MainActivity中的代码，添加按钮的点击事件如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     ... 
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     button.setOnClickListener { 
     val intent = Intent("com.example.broadcasttest.MY_BROADCAST") 
     intent.setPackage(packageName) 
     sendBroadcast(intent) //使用Intent发送，将隐式变成显示广播才能发送
     } 
     ... 
     } 
     ... 
    }
    —————————————————————————————————————————————————————————————————————————————

## 四，发送有序广播
    在上面一节的基础上
### 1，再创建一个Receiver
    创建一个新的BroadcastReceiver。新建AnotherBroadcastReceiver，代码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class AnotherBroadcastReceiver : BroadcastReceiver() { 
     
     override fun onReceive(context: Context, intent: Intent) { 
     Toast.makeText(context, "received in AnotherBroadcastReceiver", 
     Toast.LENGTH_SHORT).show() 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 2，注册接收信息
    然后在AndroidManifest.xml中对这个BroadcastReceiver的配置进行修改，代码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" 
     package="com.example.broadcasttest"> 
     ... 
     <application 
     android:allowBackup="true" 
     android:icon="@mipmap/ic_launcher" 
     android:label="@string/app_name" 
     android:roundIcon="@mipmap/ic_launcher_round" 
     android:supportsRtl="true" 
     android:theme="@style/AppTheme"> 
     ... 
     <receiver 
     android:name=".AnotherBroadcastReceiver" 
     android:enabled="true" 
     android:exported="true"> 
     <intent-filter> 
     <action android:name="com.example.broadcasttest.MY_BROADCAST" /> 
     </intent-filter> 
     </receiver> 
     </application> 
    </manifest> 
    —————————————————————————————————————————————————————————————————————————————

### 3，添加sendOrderedBroadcast(intent, null) 
    重新回到BroadcastTest项目，然后修改MainActivity中的点击按钮事件的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     ... 
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     button.setOnClickListener { 
     val intent = Intent("com.example.broadcasttest.MY_BROADCAST") 
     intent.setPackage(packageName) 
     sendOrderedBroadcast(intent, null) //唯一改变
     } 
     ... 
     } 
     ... 
    } 
    —————————————————————————————————————————————————————————————————————————————

### 4，注册设置优先性
    设定BroadcastReceiver的先后顺序，修改AndroidManifest.xml中的代码
    —————————————————————————————————————————————————————————————————————————————
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" 
     package="com.example.broadcasttest"> 
     ... 
     <application 
     android:allowBackup="true" 
     android:icon="@mipmap/ic_launcher" 
     android:label="@string/app_name" 
     android:roundIcon="@mipmap/ic_launcher_round" 
     android:supportsRtl="true" 
     android:theme="@style/AppTheme"> 
     ... 
     <receiver 
     android:name=".MyBroadcastReceiver" 
     android:enabled="true" 
     android:exported="true"> 
     <intent-filter android:priority="100"> 
     <action android:name="com.example.broadcasttest.MY_BROADCAST"/> 
     </intent-filter> 
     </receiver> 
     ... 
     </application> 
    </manifest> 
    —————————————————————————————————————————————————————————————————————————————

### 5，设置截断广播的方法
    选择是否允许广播继续传递了。修改MyBroadcastReceiver中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MyBroadcastReceiver : BroadcastReceiver() { 
     
     override fun onReceive(context: Context, intent: Intent) { 
     Toast.makeText(context, "received in MyBroadcastReceiver", 
     Toast.LENGTH_SHORT).show() 
     abortBroadcast() 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

## 五，实现强制下线功能
    缺少实践机会，下次吧，先找时间学好后面的内容先
### 1，
    先创建一个ActivityCollector类用于管理所有的Activity，代码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    object ActivityCollector { 
     
     private val activities = ArrayList<Activity>() 
     
     fun addActivity(activity: Activity) { 
     activities.add(activity) 
     } 
     
     fun removeActivity(activity: Activity) { 
     activities.remove(activity) 
     } 
     
    fun finishAll() {
        val copy = ArrayList(activities) // 创建副本
        for (activity in copy) {
            if (!activity.isFinishing) {
                activity.finish()
            }
        }
        activities.clear()
    }
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 2，
    然后创建BaseActivity类作为所有Activity的父类，代码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    open class BaseActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     ActivityCollector.addActivity(this) 
     } 
     
     override fun onDestroy() { 
     super.onDestroy() 
     ActivityCollector.removeActivity(this) 
     } 
     
    } 
    —————————————————————————————————————————————————————————————————————————————

### 3，
    然后编辑布局文件activity_login.xml
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <LinearLayout 
     android:orientation="horizontal" 
     android:layout_width="match_parent" 
     android:layout_height="60dp"> 
     <TextView 
     android:layout_width="90dp" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center_vertical" 
     android:textSize="18sp" 
     android:text="Account:" /> 
     
     <EditText 
     android:id="@+id/accountEdit" 
     android:layout_width="0dp" 
     android:layout_height="wrap_content" 
     android:layout_weight="1" 
     android:layout_gravity="center_vertical" /> 
     </LinearLayout> 
     
     <LinearLayout 
     android:orientation="horizontal" 
     android:layout_width="match_parent" 
     android:layout_height="60dp"> 
     <TextView 
     android:layout_width="90dp" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center_vertical" 
     android:textSize="18sp" 
     android:text="Password:" /> 
     
     <EditText 
     android:id="@+id/passwordEdit" 
     android:layout_width="0dp" 
     android:layout_height="wrap_content" 
     android:layout_weight="1" 
     android:layout_gravity="center_vertical" 
     android:inputType="textPassword" /> 
     </LinearLayout> 
     
     <Button 
     android:id="@+id/login" 
     android:layout_width="200dp" 
     android:layout_height="60dp" 
     android:layout_gravity="center_horizontal" 
     android:text="Login" /> 
     
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————

### 4，
    接下来修改LoginActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class LoginActivity : BaseActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_login) 
     login.setOnClickListener { 
     val account = accountEdit.text.toString() 
     val password = passwordEdit.text.toString() 
     // 如果账号是admin且密码是123456，就认为登录成功 
     if (account == "admin" && password == "123456") { 
     val intent = Intent(this, MainActivity::class.java) 
     startActivity(intent) 
     finish() 
     } else { 
     Toast.makeText(this, "account or password is invalid", 
     Toast.LENGTH_SHORT).show() 
     } 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 5，
    修改activity_main.xml中的代码
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <Button 
     android:id="@+id/forceOffline" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Send force offline broadcast" /> 
     
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 6，
    然后修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : BaseActivity() {
    override fun onResume() {
        super.onResume()
        // 关键修复：将按钮点击事件注册移到 onResume() 中
        forceOffline.setOnClickListener {
            val intent = Intent("com.example.broadcastbestpractice.FORCE_OFFLINE")
            sendBroadcast(intent)
        }
     }
    }
    —————————————————————————————————————————————————————————————————————————————

### 7，
    修改BaseActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    open class BaseActivity : AppCompatActivity() { 
     
     lateinit var receiver: ForceOfflineReceiver 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     ActivityCollector.addActivity(this) 
     } 
     
     override fun onResume() { 
     super.onResume() 
     val intentFilter = IntentFilter() 
     intentFilter.addAction("com.example.broadcastbestpractice.FORCE_OFFLINE") 
     receiver = ForceOfflineReceiver() 
     registerReceiver(receiver, intentFilter) 
     } 
     
     override fun onPause() { 
     super.onPause() 
     unregisterReceiver(receiver) 
     } 
     
     override fun onDestroy() { 
     super.onDestroy() 
     ActivityCollector.removeActivity(this) 
     } 
     
     inner class ForceOfflineReceiver : BroadcastReceiver() { 
     
     override fun onReceive(context: Context, intent: Intent) { 
     Log.d("ForceOffline", "Broadcast received!") // 添加日志
     AlertDialog.Builder(context).apply { 
     setTitle("Warning") 
     setMessage("You are forced to be offline. Please try to login again.") 
     setCancelable(false) 
     setPositiveButton("OK") { _, _ -> 
     ActivityCollector.finishAll() // 销毁所有Activity 
     val i = Intent(context, LoginActivity::class.java) 
     context.startActivity(i) // 重新启动LoginActivity 
     } 
     show() 
     } 
     } 
     
     } 
     
    } 
    —————————————————————————————————————————————————————————————————————————————

### 8，
    接下来我们还需要对AndroidManifest.xml文件进行修改
    —————————————————————————————————————————————————————————————————————————————
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" 
     package="com.example.broadcastbestpractice"> 
     <application 
     android:allowBackup="true" 
     android:icon="@mipmap/ic_launcher" 
     android:label="@string/app_name" 
     android:roundIcon="@mipmap/ic_launcher_round" 
     android:supportsRtl="true" 
     android:theme="@style/AppTheme"> 
     <activity android:name=".LoginActivity"> 
     <intent-filter> 
     <action android:name="android.intent.action.MAIN"/> 
     <category android:name="android.intent.category.LAUNCHER"/> 
     </intent-filter> 
     </activity> 
     
     <activity android:name=".MainActivity"> 
     </activity> 
     </application> 
    </manifest> 
    —————————————————————————————————————————————————————————————————————————————

# 《五》数据存储全方案，详解持久化技术

## 一，将数据存储到文件中

### 1,编写save函数示例
    以下是一段简单的代码示例，展示了如何将一段文本内容保存到文件中，通用模板:
    —————————————————————————————————————————————————————————————————————————————
    fun save(inputText: String) { 
     try { 
     val output = openFileOutput("data", Context.MODE_PRIVATE) //获取FileOutputSream对象
     val writer = BufferedWriter(OutputStreamWriter(output)) 
     writer.use { 
     it.write(inputText) 
     } 
     } catch (e: IOException) { 
     e.printStackTrace() 
     } 
    } //比较像java流，看不懂就去记忆
    —————————————————————————————————————————————————————————————————————————————

### 2,添加EditText
    先创建一个FilePersistenceTest项目，并修改activity_main.xml中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <EditText 
     android:id="@+id/editText" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:hint="Type something here" 
     /> 
     
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 3,调用示例
    修改MainActivity中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     } 
     
     override fun onDestroy() { 
     super.onDestroy() 
     val inputText = editText.text.toString() //获取输入字符串内容
     save(inputText) //调用save
     } 
     
     //编写save函数
     private fun save(inputText: String) { 
     try { 
     val output = openFileOutput("data", Context.MODE_PRIVATE) 
     val writer = BufferedWriter(OutputStreamWriter(output)) 
     writer.use { 
     it.write(inputText) 
     } 
     } catch (e: IOException) { 
     e.printStackTrace() 
     } 
     } 
    } 
    —————————————————————————————————————————————————————————————————————————————
    验证：
    我们可以借助Device File Explorer工具查看一下。这个工具在Android
    Studio的右侧边栏当中，通常是在右下角的位置，如果你的右侧边栏中没有这个工具的话，也
    可以使用快捷键Ctrl + Shift + A（Mac系统是command + shift + A）打开搜索功能，在搜
    索框中输入“Device File Explorer”即可找到这个工具。
    这个工具其实就相当于一个设备文件浏览器，我们在这里找
    到/data/data/com.example.ﬁlepersistencetest/ﬁles/目录，可以看到，现在已经生成了一
    个data文件

## 二，从文件中读取数据
    继续上一小节的代码
### 1，编写load函数               
    以下是一段简单的代码示例，展示了如何从文件中读取文本数据：
    —————————————————————————————————————————————————————————————————————————————
    fun load(): String { 
     val content = StringBuilder()
     try { 
     val input = openFileInput("data") 
     val reader = BufferedReader(InputStreamReader(input)) 
     reader.use { 
     reader.forEachLine { 
     content.append(it) 
     } 
     } 
     } catch (e: IOException) { 
     e.printStackTrace() 
     } 
     return content.toString() //暂时死记吧
    —————————————————————————————————————————————————————————————————————————————

### 2，使用示例
    继续完善上一小节中的例子，使得重新启动程序时EditText中能够保留我们上次输入的内容。
    修改MainActivity中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     //调用load函数
     val inputText = load() 
     if (inputText.isNotEmpty()) { 
     editText.setText(inputText) 
     editText.setSelection(inputText.length) 
     Toast.makeText(this, "Restoring succeeded", Toast.LENGTH_SHORT).show() 
     } 
     } 
     
     private fun load(): String { 
     val content = StringBuilder() 
     try { 
     val input = openFileInput("data") 
     val reader = BufferedReader(InputStreamReader(input)) 
     reader.use { 
     reader.forEachLine { 
     content.append(it) 
     } 
     } 
     } catch (e: IOException) { 
     e.printStackTrace() 
     } 
     return content.toString() 
     } 
     ... 
    } 
    —————————————————————————————————————————————————————————————————————————————

## 三，SharedPreferences存储和读取数据

### 1，添加保存按钮
    现在实现储存数据的功能
    新建一个SharedPreferencesTest项目，然后修改activity_main.xml中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" 
     android:orientation="vertical" > 
     
     <Button 
     android:id="@+id/saveButton" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Save Data" 
     /> 
     
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 2，编辑保存事件
    然后修改MainActivity中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     saveButton.setOnClickListener { 
     val editor = getSharedPreferences("data", Context.MODE_PRIVATE).edit() 
     editor.putString("name", "Tom") 
     editor.putInt("age", 28) 
     editor.putBoolean("married", false) 
     editor.apply() 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 3，添加读取按钮
    再实现读取数据的功能
    仍然是在SharedPreferencesTest项目的基础上继续开发，修改activity_main.xml中的代码：
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" 
     android:orientation="vertical" > 
     
     <Button 
     android:id="@+id/saveButton" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Save Data" 
     /> 
     
     <Button 
     android:id="@+id/restoreButton" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Restore Data" 
     /> 
     
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 4，编辑读取事件
    修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     ... 
     restoreButton.setOnClickListener { 
     val prefs = getSharedPreferences("data", Context.MODE_PRIVATE) 
     val name = prefs.getString("name", "") 
     val age = prefs.getInt("age", 0) 
     val married = prefs.getBoolean("married", false) 
     Log.d("MainActivity", "name is $name") 
     Log.d("MainActivity", "age is $age") 
     Log.d("MainActivity", "married is $married") 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

## 四，SharedPreferences实现记住密码功能

### 1，添加复选框按键
    复用上一章的最佳实践的登录界面的代码
    那就首先打开BroadcastBestPractice项目，编辑一下登录界面的布局。
    修改activity_login.xml中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     ... 
     
     <LinearLayout 
     android:orientation="horizontal" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content"> 
     
     <CheckBox 
     android:id="@+id/rememberPass" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" /> 
     
     <TextView 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:textSize="18sp" 
     android:text="Remember password" /> 
     
     </LinearLayout> 
     
     <Button 
     android:id="@+id/login" 
     android:layout_width="match_parent" 
     android:layout_height="60dp" 
     android:text="Login" /> 
     
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————

### 2，添加记住密码的逻辑
    然后修改LoginActivity中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class LoginActivity : BaseActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_login) 
     val prefs = getPreferences(Context.MODE_PRIVATE) 
     val isRemember = prefs.getBoolean("remember_password", false) 
     if (isRemember) { 
     // 将账号和密码都设置到文本框中 
     val account = prefs.getString("account", "") 
     val password = prefs.getString("password", "") 
     accountEdit.setText(account) 
     passwordEdit.setText(password) 
     rememberPass.isChecked = true 
     } 
     login.setOnClickListener { 
     val account = accountEdit.text.toString() 
     val password = passwordEdit.text.toString() 
     // 如果账号是admin且密码是123456，就认为登录成功 
     if (account == "admin" && password == "123456") { 
     val editor = prefs.edit() 
     if (rememberPass.isChecked) { // 检查复选框是否被选中 
     editor.putBoolean("remember_password", true) 
     editor.putString("account", account) 
     editor.putString("password", password) 
     } else { 
     editor.clear() 
     } 
     editor.apply() 
     val intent = Intent(this, MainActivity::class.java) 
     startActivity(intent) 
     finish() 
     } else { 
     Toast.makeText(this, "account or password is invalid", 
     Toast.LENGTH_SHORT).show() 
     } 
     } 
     } 
     
    } 
    —————————————————————————————————————————————————————————————————————————————
    这里只是一个简单的例子，实际项目中不会这样使用，但是里面的条件判断逻辑有点难度

## 五，创建数据库

### 1.创建自己的数据库类
    新建MyDatabaseHelper类继承自SQLiteOpenHelper，代码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MyDatabaseHelper(val context: Context, name: String, version: Int) : 
     SQLiteOpenHelper(context, name, null, version) { 
     
    //看不懂就去补全SQL知识
     private val createBook = "create table Book (" + 
     " id integer primary key autoincrement," + 
     "author text," + 
     "price real," + 
     "pages integer," + 
     "name text)" 
     
     override fun onCreate(db: SQLiteDatabase) { 
     db.execSQL(createBook) 
     Toast.makeText(context, "Create succeeded", Toast.LENGTH_SHORT).show() 
     } 
     
     override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) { 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 2，创建点击按钮
    现在修改activity_main.xml中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" 
     > 
     
     <Button 
     android:id="@+id/createDatabase" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Create Database" 
     /> 
     
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————

### 3，编辑创建事件
    最后修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val dbHelper = MyDatabaseHelper(this, "BookStore.db", 1) 
     createDatabase.setOnClickListener { 
     dbHelper.writableDatabase 
     } 
     } 
     
    } 
    —————————————————————————————————————————————————————————————————————————————
    证明成功创建的方法可见书本的详细内容

## 六，升级数据库
    上一节的代码延续
### 1，添加创建逻辑，但还是没有成功
    目前，DatabaseTest项目中已经有一张Book表用于存放书的各种详细数据
    接下来我们将这条建表语句添加到MyDatabaseHelper中，代码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MyDatabaseHelper(val context: Context, name: String, version: Int): 
     SQLiteOpenHelper(context, name, null, version) { 
     ... 
     private val createCategory = "create table Category (" + 
     "id integer primary key autoincrement," + 
     "category_name text," + 
     "category_code integer)" 
     
     override fun onCreate(db: SQLiteDatabase) { 
     db.execSQL(createBook) 
     db.execSQL(createCategory) 
     Toast.makeText(context, "Create succeeded", Toast.LENGTH_SHORT).show() 
     } 
     
     override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) { 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 2，修改更新函数
    通过卸载程序的方式来新增一张表毫无疑问是很极端的做法，其实我们只需要巧妙地运
    用SQLiteOpenHelper的升级功能，就可以很轻松地解决这个问题。修改 MyDatabaseHelper中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MyDatabaseHelper(val context: Context, name: String, version: Int): 
     SQLiteOpenHelper(context, name, null, version) { 
     ... 
     override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) { 
     db.execSQL("drop table if exists Book") 
     db.execSQL("drop table if exists Category") 
     onCreate(db) 
     } 
     
    } 
    —————————————————————————————————————————————————————————————————————————————

### 3，增加版本信息
    修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val dbHelper = MyDatabaseHelper(this, "BookStore.db", 2) //高版本会触发更新函数的启动
     createDatabase.setOnClickListener { 
     dbHelper.writableDatabase 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

## 七，数据操作CRUD

### 1，添加按钮
    向数据库的表中添加数据，继续打开上一节的项目
    修改activity_main.xml中的代码
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" 
     > 
     
     ... 
     
     <Button 
     android:id="@+id/addData" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Add Data" 
     /> 
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 2，添加示例
    接着修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val dbHelper = MyDatabaseHelper(this, "BookStore.db", 2) 
     ... 
     addData.setOnClickListener { 
     val db = dbHelper.writableDatabase 
     val values1 = ContentValues().apply { 
     // 开始组装第一条数据 
     put("name", "The Da Vinci Code") 
     put("author", "Dan Brown") 
     put("pages", 454) 
     put("price", 16.96) 
     } 
     db.insert("Book", null, values1) // 插入第一条数据 
     val values2 = ContentValues().apply { 
     // 开始组装第二条数据 
     put("name", "The Lost Symbol") 
     put("author", "Dan Brown") 
     put("pages", 510) 
     put("price", 19.95) 
     } 
     db.insert("Book", null, values2) // 插入第二条数据 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 3,更新按钮
    我们仍然是在DatabaseTest项目的基础上修改
    首先修改activity_main.xml中的代码
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" 
     > 
     
     ... 
     
     <Button 
     android:id="@+id/updateData" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Update Data" 
     /> 
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 4，更新示例
    然后修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val dbHelper = MyDatabaseHelper(this, "BookStore.db", 2) 
     ... 
     updateData.setOnClickListener { 
     val db = dbHelper.writableDatabase 
     val values = ContentValues() 
     values.put("price", 10.99) 
     db.update("Book", values, "name = ?", arrayOf("The Da Vinci Code")) 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 5，删除按钮
    继续修改activity_main.xml中的代码
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" 
     > 
     
     ... 
     
     <Button 
     android:id="@+id/deleteData" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Delete Data" 
     /> 
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————

### 6，删除示例
    然后修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val dbHelper = MyDatabaseHelper(this, "BookStore.db", 2) 
     ... 
     deleteData.setOnClickListener { 
     val db = dbHelper.writableDatabase 
     db.delete("Book", "pages > ?", arrayOf("500")) 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 7，查询按钮
    继续修改activity_main.xml中的代码
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" 
     > 
     
     ... 
     
     <Button 
     android:id="@+id/queryData" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Query Data" 
     /> 
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————

### 8，查询示例
    然后修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val dbHelper = MyDatabaseHelper(this, "BookStore.db", 2) 
     ... 
     queryData.setOnClickListener { 
     val db = dbHelper.writableDatabase 
     // 查询Book表中所有的数据 
     val cursor = db.query("Book", null, null, null, null, null, null) 
     if (cursor.moveToFirst()) { 
     do { 
     // 遍历Cursor对象，取出数据并打印 
     val name = cursor.getString(cursor.getColumnIndex("name")) 
     val author = cursor.getString(cursor.getColumnIndex("author")) 
     val pages = cursor.getInt(cursor.getColumnIndex("pages")) 
     val price = cursor.getDouble(cursor.getColumnIndex("price")) 
     Log.d("MainActivity", "book name is $name") 
     Log.d("MainActivity", "book author is $author") 
     Log.d("MainActivity", "book pages is $pages") 
     Log.d("MainActivity", "book price is $price") 
     } while (cursor.moveToNext()) 
     } 
     cursor.close() 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

## 八，使用SQL操作数据库

### 可替换前面的方法使用
    下面我就来简略演示一下，如何直接使用SQL来完成前面几个小节中学过的CRUD操作。

    添加数据：
    db.execSQL("insert into Book (name, author, pages, price) values(?, ?, ?, ?)", 
     arrayOf("The Da Vinci Code", "Dan Brown", "454", "16.96") 
    ) 
    db.execSQL("insert into Book (name, author, pages, price) values(?, ?, ?, ?)", 
     arrayOf("The Lost Symbol", "Dan Brown", "510", "19.95") 
    ) 

    更新数据：
    db.execSQL("update Book set price = ? where name = ?", arrayOf("10.99", "The Da Vinci Code"))

    删除数据：
    db.execSQL("delete from Book where pages > ?", arrayOf("500")) 

    查询数据：
    val cursor = db.rawQuery("select * from Book", null)

## 九，实践使用

### 1，
    修改activity_main.xml中的代码
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" 
     android:orientation="vertical" > 
     
     ... 
     
     <Button 
     android:id="@+id/replaceData" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Replace Data" 
     /> 
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 2，
    然后修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val dbHelper = MyDatabaseHelper(this, "BookStore.db", 2) 
     ... 
     replaceData.setOnClickListener { 
     val db = dbHelper.writableDatabase 
     db.beginTransaction() // 开启事务 
     try { 
     db.delete("Book", null, null) 
     if (true) { 
     // 手动抛出一个异常，让事务失败 
     throw NullPointerException() 
     } 
     val values = ContentValues().apply { 
     put("name", "Game of Thrones") 
     put("author", "George Martin") 
     put("pages", 720) 
     put("price", 20.85) 
     } 
     db.insert("Book", null, values) 
     db.setTransactionSuccessful() // 事务已经执行成功 
     } catch (e: Exception) { 
     e.printStackTrace() 
     } finally { 
     db.endTransaction() // 结束事务 
     } 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

## 十，升级数据库的最佳写法

### 1，
    MyDatabaseHelper中的代码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MyDatabaseHelper(val context: Context, name: String, version: Int): 
     SQLiteOpenHelper(context, name, null, version) { 
     
     private val createBook = "create table Book (" + 
     " id integer primary key autoincrement," + 
     "author text," + 
     "price real," + 
     "pages integer," + 
     "name text)" 
     
     override fun onCreate(db: SQLiteDatabase) { 
     db.execSQL(createBook) 
     } 
     
     override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) { 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 2，
    修改MyDatabaseHelper中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MyDatabaseHelper(val context: Context, name: String, version: Int): 
     SQLiteOpenHelper(context, name, null, version) { 
     
     private val createBook = "create table Book (" + 
     " id integer primary key autoincrement," + 
     "author text," + 
     "price real," + 
     "pages integer," + 
     "name text)" 
     
     private val createCategory = "create table Category (" + 
     "id integer primary key autoincrement," + 
     "category_name text," + 
     "category_code integer)" 
     
     override fun onCreate(db: SQLiteDatabase) { 
     db.execSQL(createBook) 
     db.execSQL(createCategory) 
     } 
     
     override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) { 
     if (oldVersion <= 1) { 
     db.execSQL(createCategory) 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 3，
    再次修改MyDatabaseHelper中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MyDatabaseHelper(val context: Context, name: String, version: Int): 
     SQLiteOpenHelper(context, name, null, version) { 
     
     private val createBook = "create table Book (" + 
     " id integer primary key autoincrement," + 
     "author text," + 
     "price real," + 
     "pages integer," + 
     "name text," + 
     "category_id integer)" 
     
     private val createCategory = "create table Category (" + 
     "id integer primary key autoincrement," + 
     "category_name text," + 
     "category_code integer)" 
     
     override fun onCreate(db: SQLiteDatabase) { 
     db.execSQL(createBook) 
     db.execSQL(createCategory) 
     } 
     
     override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) { 
     if (oldVersion <= 1) { 
     db.execSQL(createCategory) 
     } 
     if (oldVersion <= 2) { 
     db.execSQL("alter table Book add column category_id integer") 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

# 《六》探究 ContentProvider

## 一，在程序运行时申请权限

### 1，编辑按钮
    首先新建一个RuntimePermissionTest项目
    使用CALL_PHONE这个权限来作为本小节的示例
    修改activity_main.xml布局文件
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <Button 
     android:id="@+id/makeCall" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Make Call" /> 
     
    </LinearLayout> 
     —————————————————————————————————————————————————————————————————————————————

### 2，编辑点击事件
    接着修改MainActivity中的代码
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     makeCall.setOnClickListener { 
     //异常处理
     try { 
     val intent = Intent(Intent.ACTION_CALL) 
     intent.data = Uri.parse("tel:10086") 
     startActivity(intent) 
     } catch (e: SecurityException) { 
     e.printStackTrace() 
     } 
     } 
     }  
     
    } 
    —————————————————————————————————————————————————————————————————————————————

### 3，声明权限
    接下来修改AndroidManifest.xml文件，在其中声明如下权限：
    —————————————————————————————————————————————————————————————————————————————
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" 
     package="com.example.runtimepermissiontest"> 
     
     <uses-permission android:name="android.permission.CALL_PHONE" /> 
     
     <application 
     android:allowBackup="true" 
     android:icon="@mipmap/ic_launcher" 
     android:label="@string/app_name" 
     android:roundIcon="@mipmap/ic_launcher_round" 
     android:supportsRtl="true" 
     android:theme="@style/AppTheme"> 
     ... 
     </application> 
     
    </manifest> 
    —————————————————————————————————————————————————————————————————————————————

### 4，修改后的编辑逻辑
    使用危险权限时必须进行运行时权限处理。
    那么下面我们就来尝试修复这个问题，修改MainActivity中的代码，
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main)  
     makeCall.setOnClickListener { 
     if (ContextCompat.checkSelfPermission(this, 
     Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) { 
     //检查是否授权
     ActivityCompat.requestPermissions(this, 
     arrayOf(Manifest.permission.CALL_PHONE), 1) 
     } else { 
     //如果授权了就直接拨号
     call() 
     } 
     } 
     } 
     
     override fun onRequestPermissionsResult(requestCode: Int, 
     permissions: Array<String>, grantResults: IntArray) { 
     super.onRequestPermissionsResult(requestCode, permissions, grantResults) 
     when (requestCode) { 
     1 -> { // 对应上面 requestPermissions 的 requestCode=1
     if (grantResults.isNotEmpty() && 
     grantResults[0] == PackageManager.PERMISSION_GRANTED) { 
     call() // 权限同意 → 拨号
     } else { 
     Toast.makeText(this, "You denied the permission", 
     Toast.LENGTH_SHORT).show() 
     } 
     } 
     } 
     } 
     
     private fun call() { 
     try { 
     val intent = Intent(Intent.ACTION_CALL) // 指定“直接拨号”动作
     intent.data = Uri.parse("tel:10086") // 设置电话号码
     startActivity(intent) // 启动拨号
     } catch (e: SecurityException) { 
     e.printStackTrace() // 本应不会触发（权限已检查）
     } 
     } 
     
    } 
     —————————————————————————————————————————————————————————————————————————————
    用户点击后，先检查“能否拨打电话”的权限，如果没权限就申请权限，申请成功后直接拨号（例如拨打10086）

## 二， ContentProvider读取系统联系人
    省略了ContentResolver的基本用法
### 1，添加ListView布局
    打开通讯录程序通过点击“Create new contact”创建联系人。
    这里就先创建两个联系人吧，分别填入他们的姓名和手机号。
    现在新建一个ContactsTest项目
    首先还是来编写一下布局文件，这里我们希望读取出来的联系人信息能够在ListView中显示，
    因此，修改activity_main.xml中的代码
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <ListView 
     android:id="@+id/contactsView" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     </ListView> 
     
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————

### 2，编辑读取逻辑
    接着修改MainActivity中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() {
     private val contactsList = ArrayList<String>() 
     private lateinit var adapter: ArrayAdapter<String> 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     adapter = ArrayAdapter(this, android.R.layout.simple_list_item_1, contactsList) 
     contactsView.adapter = adapter  // 创建一个空列表 contactsList，将列表绑定到UI
     if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS) 
     != PackageManager.PERMISSION_GRANTED) { 
     ActivityCompat.requestPermissions(this, 
     arrayOf(Manifest.permission.READ_CONTACTS), 1) 
     } else { 
     readContacts()  // 权限已有 → 直接读取联系人
     } 
     } 
     
     override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<String>, 
     grantResults: IntArray) { 
     super.onRequestPermissionsResult(requestCode, permissions, grantResults) 
     when (requestCode) { 
     1 -> { 
     if (grantResults.isNotEmpty() 
     && grantResults[0] == PackageManager.PERMISSION_GRANTED) { 
     readContacts()  // 用户同意 → 读取联系人
     } else { 
     Toast.makeText(this, "You denied the permission", 
     Toast.LENGTH_SHORT).show() 
     } 
     } 
     } 
     } 
     
     private fun readContacts() { 
     // 查询联系人数据 
     contentResolver.query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI, // 1. 指定数据来源
     null, null, null, null)?.apply { 
     while (moveToNext()) { 
     // 2. 遍历每条联系人 
     val displayName = getString(getColumnIndex( 
     ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME)) 
     // 获取联系人手机号 
     val number = getString(getColumnIndex( 
     ContactsContract.CommonDataKinds.Phone.NUMBER)) 
     contactsList.add("$displayName\n$number") // 3. 保存数据
     } 
     adapter.notifyDataSetChanged()  // 4. 刷新列表
     close()  // 5. 关闭资源
     } 
     } 
    } 
    —————————————————————————————————————————————————————————————————————————————
    这个代码实现了一个“联系人列表展示”功能：先检查“读取联系人”权限，
    授权后从系统联系人数据库读取所有联系人（姓名+号码），并显示在列表中。

### 3，声明权限
    修改AndroidManifest.xml中的代码
    —————————————————————————————————————————————————————————————————————————————
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" 
     package="com.example.contactstest"> 
     
     <uses-permission android:name="android.permission.READ_CONTACTS" /> 
     ... 
    </manifest> 
    —————————————————————————————————————————————————————————————————————————————

## 三，创建ContentProvider的步骤

### 1，
    想要实现跨程序共享数据的功能，可以通过新建一个类去继承ContentProvider的方式来实现。
    ContentProvider类中有6个抽象方法，我们在使用子类继承它的时候，
    需要将这6个方法全部重写。观察下面的代码示例：
    —————————————————————————————————————————————————————————————————————————————
    class MyProvider : ContentProvider() { 
     
     override fun onCreate(): Boolean { 
     return false 
     } 
     
     override fun query(uri: Uri, projection: Array<String>?, selection: String?, 
     selectionArgs: Array<String>?, sortOrder: String?): Cursor? { 
     return null 
     } 
     
     override fun insert(uri: Uri, values: ContentValues?): Uri? { 
     return null 
     } 
     
     override fun update(uri: Uri, values: ContentValues?, selection: String?, 
     selectionArgs: Array<String>?): Int { 
     return 0 
     } 
     
     override fun delete(uri: Uri, selection: String?, selectionArgs: Array<String>?): Int { 
     return 0 
     } 
     
     override fun getType(uri: Uri): String? { 
     return null 
     } 
     
    } 
    —————————————————————————————————————————————————————————————————————————————

### 2，

    回顾一下，一个标准的内容URI写法是：
    content://com.example.app.provider/table1 
    这就表示调用方期望访问的是com.example.app这个应用的table1表中的数据。

    除此之外，我们还可以在这个内容URI的后面加上一个id，例如：
    content://com.example.app.provider/table1/1 

    所以，一个能够匹配任意表的内容URI格式就可以写成：
    content://com.example.app.provider/* 

    一个能够匹配table1表中任意一行数据的内容URI格式就可以写成：
    content://com.example.app.provider/table1/# 

### 3，
    修改MyProvider中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MyProvider : ContentProvider() { 
     
     private val table1Dir = 0 
     private val table1Item = 1 
     private val table2Dir = 2 
     private val table2Item = 3 
     
     private val uriMatcher = UriMatcher(UriMatcher.NO_MATCH) 
     
     init { 
     uriMatcher.addURI("com.example.app.provider", "table1", table1Dir) 
     uriMatcher.addURI("com.example.app.provider ", "table1/#", table1Item) 
     uriMatcher.addURI("com.example.app.provider ", "table2", table2Dir) 
     uriMatcher.addURI("com.example.app.provider ", "table2/#", table2Item) 
     } 
     ... 
     override fun query(uri: Uri, projection: Array<String>?, selection: String?, 
     selectionArgs: Array<String>?, sortOrder: String?): Cursor? { 
     when (uriMatcher.match(uri)) { 
     table1Dir -> { 
     // 查询table1表中的所有数据 
     } 
     table1Item -> { 
     // 查询table1表中的单条数据 
     } 
     table2Dir -> { 
     // 查询table2表中的所有数据 
     } 
     table2Item -> { 
     // 查询table2表中的单条数据 
     } 
     } 
     ... 
     } 
     ... 
    } 
     —————————————————————————————————————————————————————————————————————————————

### 4，
    
    所以，对于content://com.example.app.provider/table1这个内容URI，它所对应的MIME类
    型就可以写成：
    vnd.android.cursor.dir/vnd.com.example.app.provider.table1 

    对于content://com.example.app.provider/table1/1这个内容URI，它所对应的MIME类型就
    可以写成：
    vnd.android.cursor.item/vnd.com.example.app.provider.table1 

### 5，
    现在我们可以继续完善MyProvider中的内容了，这次来实现getType()方法中的逻辑
    ————————————————————————————————————————————————————————————————————————————
    class MyProvider : ContentProvider() { 
     ... 
     override fun getType(uri: Uri) = when (uriMatcher.match(uri)) { 
     table1Dir -> "vnd.android.cursor.dir/vnd.com.example.app.provider.table1" 
     table1Item -> "vnd.android.cursor.item/vnd.com.example.app.provider.table1" 
     table2Dir -> "vnd.android.cursor.dir/vnd.com.example.app.provider.table2" 
     table2Item -> "vnd.android.cursor.item/vnd.com.example.app.provider.table2" 
     else -> null 
     } 
    } 
    —————————————————————————————————————————————————————————————————————————————

## 四，实现跨程序数据共享

### 1，
    在上一章中DatabaseTest项目的基础上继续开发，通过ContentProvider
    来给它加入外部访问接口。打开DatabaseTest项目，首先将MyDatabaseHelper中使用Toast
    弹出创建数据库成功的提示去除，因为跨程序访问时我们不能直接使用Toast。然后创建一个
    ContentProvider，右击com.example.databasetest包→New→Other→Content
    Provider，可以看到，我们将ContentProvider命名为DatabaseProvider，将authority指定为
    com.example.databasetest.provider，Exported属性表示是否允许外部程序访问我们
    的ContentProvider，Enabled属性表示是否启用这个ContentProvider。将两个属性都勾
    中，点击“Finish”完成创建。
    接着我们修改DatabaseProvider中的代码
    —————————————————————————————————————————————————————————————————————————————
    class DatabaseProvider : ContentProvider() { 
     
     private val bookDir = 0 
     private val bookItem = 1 
     private val categoryDir = 2 
     private val categoryItem = 3 
     private val authority = "com.example.databasetest.provider" 
     private var dbHelper: MyDatabaseHelper? = null 
     
     private val uriMatcher by lazy { 
     val matcher = UriMatcher(UriMatcher.NO_MATCH) 
     matcher.addURI(authority, "book", bookDir) 
     matcher.addURI(authority, "book/#", bookItem) 
     matcher.addURI(authority, "category", categoryDir) 
     matcher.addURI(authority, "category/#", categoryItem) 
     matcher 
     } 
     
     override fun onCreate() = context?.let { 
     dbHelper = MyDatabaseHelper(it, "BookStore.db", 2) 
     true 
     } ?: false  
     
     override fun query(uri: Uri, projection: Array<String>?, selection: String?, 
     selectionArgs: Array<String>?, sortOrder: String?) = dbHelper?.let { 
     // 查询数据 
     val db = it.readableDatabase 
     val cursor = when (uriMatcher.match(uri)) { 
     bookDir -> db.query("Book", projection, selection, selectionArgs, 
     null, null, sortOrder) 
     bookItem -> { 
     val bookId = uri.pathSegments[1] 
     db.query("Book", projection, "id = ?", arrayOf(bookId), null, null, 
     sortOrder) 
     } 
     categoryDir -> db.query("Category", projection, selection, selectionArgs, 
     null, null, sortOrder) 
     categoryItem -> { 
     val categoryId = uri.pathSegments[1] 
     db.query("Category", projection, "id = ?", arrayOf(categoryId), 
     null, null, sortOrder) 
     } 
     else -> null 
     } 
     cursor 
     } 
     
     override fun insert(uri: Uri, values: ContentValues?) = dbHelper?.let { 
     // 添加数据 
     val db = it.writableDatabase 
     val uriReturn = when (uriMatcher.match(uri)) { 
     bookDir, bookItem -> { 
     val newBookId = db.insert("Book", null, values) 
     Uri.parse("content://$authority/book/$newBookId") 
     } 
     categoryDir, categoryItem -> { 
     val newCategoryId = db.insert("Category", null, values) 
     Uri.parse("content://$authority/category/$newCategoryId") 
     } 
     else -> null 
     } 
     uriReturn 
     } 
     
     override fun update(uri: Uri, values: ContentValues?, selection: String?, 
     selectionArgs: Array<String>?) = dbHelper?.let { 
     // 更新数据 
     val db = it.writableDatabase 
     val updatedRows = when (uriMatcher.match(uri)) { 
     bookDir -> db.update("Book", values, selection, selectionArgs) 
     bookItem -> { 
     val bookId = uri.pathSegments[1] 
     db.update("Book", values, "id = ?", arrayOf(bookId)) 
     } 
     categoryDir -> db.update("Category", values, selection, selectionArgs) 
     categoryItem -> { 
     val categoryId = uri.pathSegments[1] 
     db.update("Category", values, "id = ?", arrayOf(categoryId)) 
     } 
     else -> 0 
     } 
     updatedRows 
     } ?: 0 
     
     override fun delete(uri: Uri, selection: String?, selectionArgs: Array<String>?) 
     = dbHelper?.let { 
     // 删除数据  
     val db = it.writableDatabase 
     val deletedRows = when (uriMatcher.match(uri)) { 
     bookDir -> db.delete("Book", selection, selectionArgs) 
     bookItem -> { 
     val bookId = uri.pathSegments[1] 
     db.delete("Book", "id = ?", arrayOf(bookId)) 
     } 
     categoryDir -> db.delete("Category", selection, selectionArgs) 
     categoryItem -> { 
     val categoryId = uri.pathSegments[1] 
     db.delete("Category", "id = ?", arrayOf(categoryId)) 
     } 
     else -> 0 
     } 
     deletedRows 
     } ?: 0 
     
     override fun getType(uri: Uri) = when (uriMatcher.match(uri)) { 
     bookDir -> "vnd.android.cursor.dir/vnd.com.example.databasetest.provider.book" 
     bookItem -> "vnd.android.cursor.item/vnd.com.example.databasetest.provider.book" 
     categoryDir -> "vnd.android.cursor.dir/vnd.com.example.databasetest. 
     provider.category" 
     categoryItem -> "vnd.android.cursor.item/vnd.com.example.databasetest. 
     provider.category" 
     else -> null 
     } 
    } 
     —————————————————————————————————————————————————————————————————————————————
    打开AndroidManifest.xml文件瞧一瞧，自动注册已经完成了

### 2，
    接着关闭DatabaseTest这个项目，并创建一个新项目ProviderTest，
    我们将通过这个程序去访问DatabaseTest中的数 据。
    还是先来编写一下布局文件吧，修改activity_main.xml中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <Button 
     android:id="@+id/addData" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Add To Book" /> 
     
     <Button 
     android:id="@+id/queryData" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Query From Book" /> 
     
     <Button 
     android:id="@+id/updateData" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Update Book" /> 
     
     <Button 
     android:id="@+id/deleteData" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Delete From Book" /> 
     
    </LinearLayout>
    —————————————————————————————————————————————————————————————————————————————

### 3，
    后修改MainActivity中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     var bookId: String? = null 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     addData.setOnClickListener { 
     // 添加数据 
     val uri = Uri.parse("content://com.example.databasetest.provider/book") 
     val values = contentValuesOf("name" to "A Clash of Kings", 
     "author" to "George Martin", "pages" to 1040, "price" to 22.85) 
     val newUri = contentResolver.insert(uri, values) 
     bookId = newUri?.pathSegments?.get(1) 
     } 
     queryData.setOnClickListener { 
     // 查询数据 
     val uri = Uri.parse("content://com.example.databasetest.provider/book")  
     contentResolver.query(uri, null, null, null, null)?.apply { 
     while (moveToNext()) { 
     val name = getString(getColumnIndex("name")) 
     val author = getString(getColumnIndex("author")) 
     val pages = getInt(getColumnIndex("pages")) 
     val price = getDouble(getColumnIndex("price")) 
     Log.d("MainActivity", "book name is $name") 
     Log.d("MainActivity", "book author is $author") 
     Log.d("MainActivity", "book pages is $pages") 
     Log.d("MainActivity", "book price is $price") 
     } 
     close() 
     } 
     } 
     updateData.setOnClickListener { 
     // 更新数据 
     bookId?.let { 
     val uri = Uri.parse("content://com.example.databasetest.provider/ 
     book/$it") 
     val values = contentValuesOf("name" to "A Storm of Swords", 
     "pages" to 1216, "price" to 24.05) 
     contentResolver.update(uri, values, null, null) 
     } 
     } 
     deleteData.setOnClickListener { 
     // 删除数据 
     bookId?.let { 
     val uri = Uri.parse("content://com.example.databasetest.provider/ 
     book/$it") 
     contentResolver.delete(uri, null, null) 
     } 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

# 《七》运用手机多媒体

## 一，创建通知渠道

### 1，
    首先需要一个NotiﬁcationManager对通知进行管理，可以通过调用Context的
    getSystemService()方法获取。getSystemService()方法接收一个字符串参数用于确定
    获取系统的哪个服务，这里我们传入Context.NOTIFICATION_SERVICE即可。因此，获取
    NotiﬁcationManager的实例就可以写成：
    —————————————————————————————————————————————————————————————————————————————
    val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager 
    —————————————————————————————————————————————————————————————————————————————

### 2，
    接下来要使用NotificationChannel类构建一个通知渠道，并调用NotiﬁcationManager的
    createNotificationChannel()方法完成创建。由于NotificationChannel类和
    createNotificationChannel()方法都是Android 8.0系统中新增的API，因此我们在使用
    的时候还需要进行版本判断才可以，写法如下：
    —————————————————————————————————————————————————————————————————————————————
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) { 
     val channel = NotificationChannel(channelId, channelName, importance) 
     manager.createNotificationChannel(channel) 
    } 
     —————————————————————————————————————————————————————————————————————————————

## 二，通知的基本用法

### 1，
    首先需要使用一个Builder构造器来创建Notification对象，但问题在于，Android系统的每
    一个版本都会对通知功能进行或多或少的修改，API不稳定的问题在通知上凸显得尤其严重，比
    方说刚刚介绍的通知渠道功能在Android 8.0系统之前就是没有的。那么该如何解决这个问题
    呢？其实解决方案我们之前已经见过好几回了，就是使用AndroidX库中提供的兼容API。
    AndroidX库中提供了一个NotificationCompat类，使用这个类的构造器创建
    Notification对象，就可以保证我们的程序在所有Android系统版本上都能正常工作了，代
    码如下所示：
    —————————————————————————————————————————————————————————————————————————————
    val notification = NotificationCompat.Builder(context, channelId).build()
    —————————————————————————————————————————————————————————————————————————————

### 2，
    我们可以在最终的build()方法之前连缀任意多的设置方法来创建一个丰富的Notification对象，
    先来看一些最基本的设置：
    —————————————————————————————————————————————————————————————————————————————
    val notification = NotificationCompat.Builder(context, channelId) 
     .setContentTitle("This is content title")  
     .setContentText("This is content text") 
     .setSmallIcon(R.drawable.small_icon) 
     .setLargeIcon(BitmapFactory.decodeResource(getResources(),R.drawable.large_icon)) 
     .build() 
    —————————————————————————————————————————————————————————————————————————————

### 3，
    以上工作都完成之后，只需要调用NotiﬁcationManager的notify()方法就可以让通知显示
    出来了。notify()方法接收两个参数：第一个参数是id，要保证为每个通知指定的id都是不
    同的；第二个参数则是Notification对象，这里直接将我们刚刚创建好的Notification对
    象传入即可。因此，显示一个通知就可以写成：
     —————————————————————————————————————————————————————————————————————————————
    manager.notify(1, notification)
    —————————————————————————————————————————————————————————————————————————————

### 4，
    新建一个NotiﬁcationTest项目，并修改activity_main.xml中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <Button 
     android:id="@+id/sendNotice" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:text="Send Notice" /> 
     
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————

### 5，
    接下来修改MainActivity中的代码，如下所示：
    —————————————————————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     setContentView(R.layout.activity_main) 
     val manager = getSystemService(Context.NOTIFICATION_SERVICE) as 
     NotificationManager 
     if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) { 
     val channel = NotificationChannel("normal", "Normal",NotificationManager. 
     IMPORTANCE_DEFAULT) 
     manager.createNotificationChannel(channel) 
     } 
     sendNotice.setOnClickListener { 
     val notification = NotificationCompat.Builder(this, "normal") 
     .setContentTitle("This is content title")  
     .setContentText("This is content text") 
     .setSmallIcon(R.drawable.small_icon) 
     .setLargeIcon(BitmapFactory.decodeResource(resources, 
     R.drawable.large_icon)) 
     .build() 
     manager.notify(1, notification) 
     } 
     } 
     
    }
    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

## 三，

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

## 

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————


## 

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————


## 

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

### 
    
    —————————————————————————————————————————————————————————————————————————————

    —————————————————————————————————————————————————————————————————————————————

# 《八》探究service

##

###

# 《九》使用网络技术

##

###

# 《十》Material Design实战

##

###

# 《十一》探究Jetpack

##

###

# 《十二》进阶高级技巧

##

###