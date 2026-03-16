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

### 1，
    先完成创建一个完整的Activity，为了让按钮可以被正常点击，可以采用使按钮处于布局的中央位置
    修改first_layout.xml，在根目录添加  android:gravity="center"
    在Button按钮处修改  android:layout_width="wrap_content"

### 2，
    修改onCreate（），在里面添加按钮的点击事件，通过代码补全添加以下代码
    ————————————————————————————————————————————————————————————
    val button1: Button = findViewById(R.id.button1) 
     button1.setOnClickListener { 
     Toast.makeText(this, "You clicked Button 1", Toast.LENGTH_SHORT).show() 
     } 
    ————————————————————————————————————————————————————————————
    再启动程序点击按钮就可以实现了

## 三，使用Menu菜单

### 1，
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

### 2，
    接着回到FirstActivity中来重写onCreateOptionsMenu()方法，
    重写方法可以使用Ctrl + O 快捷键（Mac系统是control + O）
    ————————————————————————————————————————————————————————————————
     override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        menuInflater.inflate(R.menu.main, menu)
        return true
        }
    ————————————————————————————————————————————————————————————————

### 3，
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

### 1，（通用）
    在已经创建了一个完整的Activity的基础上，
    还是右击com.example.activitytest包→New→Activity→Empty Activity，会弹出一个创建
    Activity的对话框，这次我们命名为SecondActivity，并勾选Generate Layout File，给布局
    文件起名为second_layout，但不要勾选Launcher Activity选项
    只是练习就可以按照first_layout的布局编写新的布局就可以了，
    注意删去SecondActivity的多余自生成部分，留下表示引入布局的部分就可以了

### 2，（显式）
    修改FirstAcivity文件里面的setOnClickListener（）
     ——————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val intent = Intent(this, SecondActivity::class.java) 
     startActivity(intent) 
    } 
    ——————————————————————————————————————————————————————————
    显式到这里就算完成了

### 3，（隐式）
    接下来是完成隐式编写
    先完成第一步，然后打开AndroidManifest.xml，添加如下代码：
    ——————————————————————————————————————————————————————————————————————————
    <activity android:name=".SecondActivity" > 
     <intent-filter> 
     <action android:name="com.example.activitytest.ACTION_START" /> 
     <category android:name="android.intent.category.DEFAULT" /> 
     <category android:name="com.example.activitytest.MY_CATEGORY"/>
     </intent-filter> 
    </activity>
    ——————————————————————————————————————————————————————————————————————————
    还要注意将false修改成true

### 4，（隐式）
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

### 7，（拓展）
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

### 1，
    创建好两个Activity之后，先编辑FirstActivity，添加以下的代码
     ————————————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     val data = "Hello SecondActivity" 
     val intent = Intent(this, SecondActivity::class.java) 
     intent.putExtra("extra_data", data) 
     startActivity(intent) 
    } 
    ————————————————————————————————————————————————————————————————

### 2，
    然后在SecondActivity中将传递的数据取出，并打印出来，代码如下所示：
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

### 2，SecondActivity中给按钮注册点击事件，并在点击事件中添加返回数据的逻辑
    ———————————————————————————————————————————————————————
    class SecondActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.second_layout)
            val button2: Button = findViewById(R.id.button2)
            button2.setOnClickListener {
                val intent = Intent()
                intent.putExtra("data_return", "Hello FirstActivity")
                setResult(RESULT_OK, intent)
                finish()
            }
        }
    }
    ———————————————————————————————————————————————————————

### 3，在FirstActivity中重写onActivityResult()方法
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

### 4，在SecondActivity的onCreate()添加onBackPressedDispatcher方法
    为了保销毁了Activity也可以实现传递数据
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

### 1，
    先创建一个新项目，允许系统自己创建Activity和布局
    再右击com.example.activitylifecycletest包→New→Activity→Empty Activity，新建
    NormalActivity，布局起名为normal_layout。然后使用同样的方式创建DialogActivity，布
    局起名为dialog_layout。

### 2，
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

### 3，
    修改AndroidManifest.xml的<activity>标签的配置，就是添加一行而已
    应该还记得需要将false修改成true，如下所示：
    ————————————————————————————————————————————————————————————————
    <activity android:name=".DialogActivity" 
     android:theme="@style/Theme.AppCompat.Dialog"> 
    </activity> 
    <activity android:name=".NormalActivity"> 
    </activity>
    ————————————————————————————————————————————————————————————————

### 4，
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
     android:text="Start NormalActivity" /> 
     
     <Button 
     android:id="@+id/startDialogActivity" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Start DialogActivity" /> 
     
    </LinearLayout> 
    —————————————————————————————————————————————————————————————————————————————
    但是可能由于是显示器的问题，第一个按键被放置在了最顶端无法被触碰

### 5，
    最后修改MainActivity中的代码，如下所示：
    ——————————————————————————————————————————————————————————————————————————————
    class MainActivity : ComponentActivity() {
        private val tag = "MainActivity"
    
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            Log.d(tag, "onCreate")
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
    ——————————————————————————————————————————————————————————————————————————————

## 八，数据的临时保存与恢复

### 1，
    先至少完成创造了一个完整的Activity的过程，
    在MainActivity中添加如下代码
    ————————————————————————————————————————————————————
    override fun onSaveInstanceState(outState: Bundle) { 
     super.onSaveInstanceState(outState) 
     val tempData = "Something you just typed" 
     outState.putString("data_key", tempData) 
    }
    ————————————————————————————————————————————————————

### 2，
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
    等项目驱动

## 十，知晓当前是在哪一个Activity

### 1，
    我们还是在ActivityTest项目的基础上修改，首先需要新建一个BaseActivity类。右击
    com.example.activitytest包→New→Kotlin File/Class，在弹出的窗口中输入
    BaseActivity，创建类型选择Class
    如下所示：
    ————————————————————————————————————————————————————————
    open class BaseActivity : AppCompatActivity() { 
     
     override fun onCreate(savedInstanceState: Bundle?) { 
     super.onCreate(savedInstanceState) 
     Log.d("BaseActivity", javaClass.simpleName) 
     } 
     
    }
    —————————————————————————————————————————————————————————

### 2，
    然后修改
    FirstActivity、SecondActivity和ThirdActivity的继承结构，让它们不再继承自
    AppCompatActivity，而是继承自BaseActivity。而由于BaseActivity又是继承自
    AppCompatActivity的，所以项目中所有Activity的现有功能并不受影响，它们仍然继承了
    Activity中的所有特性。

## 十一，随时随地退出程序

### 1，
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

### 2，
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

### 3，
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

### 1，
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

### 2，
    只需要一行代码就可以启动SecondActivity，如下所示：
    ————————————————————————————————————————————————————————
    button1.setOnClickListener { 
     SecondActivity.actionStart(this, "data1", "data2") 
    } 
    ————————————————————————————————————————————————————————

# 《二》UI开发的点点滴滴

## 一，使用实现接口的方式来进行注册button点击事件，并通过点击按钮获取EditText中输入的内容。

### 1，
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

### 2，
    然后修改对应的Activity中的编辑button点击事件的代码
    ————————————————————————————————————————————————————————————
    class MainActivity : AppCompatActivity(), View.OnClickListener { 
     
     override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.first_layout)
        val button1: Button = findViewById(R.id.button1)
        button1.setOnClickListener(this)
    }
     
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

### 1,
    我们在res目录下再新建一个drawable-xxhdpi目录，然后将事先准备好的两张图片img_1.png和img_2.png复制到该目录当中。
    接下来修改activity_main.xml，添加如下代码：
    ——————————————————————————————————————————————————————————————
    <ImageView 
     android:id="@+id/imageView" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:src="@drawable/img_1" 
     /> 
    ——————————————————————————————————————————————————————————————

### 2,
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

## 三，使用ProgressBar

### 1，
    先在布局中添加你想要的版本，一下是两个版本
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

### 2，
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
     } 
     } 
     } 
     } 
     
    } 
    ————————————————————————————————————————————————————————————————————

### 3，
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

### 1，
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
### 1，
    这里我提前准备好了3张图片——title_bg.png、back_bg.png和edit_bg.png（资源下载地址见
    前言），分别用于作为标题栏、返回按钮和编辑按钮的背景。
    先在layout目录下创建一个title.xml布局，填写以下代码
    ————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:background="@drawable/title_bg"> 
     
     <Button
     android:id="@+id/titleBack" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:layout_gravity="center" 
     android:layout_margin="5dp" 
     android:background="@drawable/back_bg" 
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
     android:background="@drawable/edit_bg" 
     android:text="Edit" 
     android:textColor="#fff" /> 
     
    </LinearLayout> 

    ————————————————————————————————————————————————————————————

### 2，
    接着修改activity_main.xml中的代码，如下所示：
    ————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <include layout="@layout/title" /> 
     
    </LinearLayout> 
    ——————————————————————————————————————————————————————————————————————————

### 3，
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

### 1，
    新建TitleLayout继承自LinearLayout，让它成为我们自定义的标题栏控件，代码如下所示：
    ——————————————————————————————————————————————————————————————————————————————————————————
    class TitleLayout(context: Context, attrs: AttributeSet) : LinearLayout(context, attrs) { 
     
     init { 
     LayoutInflater.from(context).inflate(R.layout.title, this) 
     } 
     
    }
    ————————————————————————————————————————————————————————————————————————————————————————————

### 2，
    接下来我们需要在布局文件中添加这个自定义控件，修改activity_main.xml中的代码，如下所示：
    ——————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent" > 
     
     <com.example.uicustomviews.TitleLayout 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" /> 
     
    </LinearLayout> 
    ——————————————————————————————————————————————————————————————————————

### 3，
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

### 1,
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

### 2,
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

## 八，定制ListView的界面
    缺点还是找不到合适的图片
### 1，
    首先需要准备好一组图片资源（资源下载地址见前言），分别对应上面提供的每一种水果，
    待会我们要让这些水果名称的旁边都有一张相应的图片。
    接着定义一个实体类，作为ListView适配器的适配类型。新建Fruit类，代码如下所示：
    ————————————————————————————————————————————————————————————————————————————
    class Fruit(val name:String, val imageId: Int)
    ————————————————————————————————————————————————————————————————————————————

### 2，
    然后需要为ListView的子项指定一个我们自定义的布局，在layout目录下新建fruit_item.xml，代码如下所示：
    ————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:layout_width="match_parent" 
     android:layout_height="60dp"> 
     
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

### 3，
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

### 4，
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

## 十，实现横向滚动和瀑布流布局

## 十一，编辑RecyclerView的点击事件

## 十二，制作9-Patch图片

## 十三，编写精美的聊天界面

# 《三》探究Fragment

