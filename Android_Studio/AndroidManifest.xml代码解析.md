 ✔
# 一，注册Activity,声明主activity ✔
    ——————————————————————————————————————————————————————————————————————
    <manifest xmlns:android="http://schemas.android.com/apk/res/android" 
     package="com.example.activitytest"> 
     
     <application 
     ...> 
     <activity android:name=".FirstActivity" 
     android:label="This is FirstActivity"> 
     <intent-filter> 
     <action android:name="android.intent.action.MAIN" /> 
     <category android:name="android.intent.category.LAUNCHER" /> 
     </intent-filter> 
     </activity> 
     </application> 
     
    </manifest>
    ——————————————————————————————————————————————————————————————————————

    AndroidManifest.xml 是 Android 应用的“说明书”，MAIN + LAUNCHER 配置决定了应用如何从桌面启动，
    而 package 和 android:name 确保系统能正确找到你的代码。
    记住：没有 MAIN + LAUNCHER，应用就是“死的”；没有正确路径，代码就是“找不到的”。

## 1. 根标签 <manifest>

    xmlns:android：
    定义 Android XML 命名空间，必须存在，用于引用 Android 系统属性（如 android:name）。

    package：
    指定应用的唯一包名（Java 包名规范）。
    作为应用的全局唯一标识（类似身份证号）。
    决定生成的 APK 文件名和资源 ID 的前缀（如 R.layout.activity_main）。
    注意：包名必须是小写字母+数字，且不能包含下划线（如 com.example.activity_test 会报错）。

## 2. 应用容器 <application>

    作用：定义应用的整体配置（如图标、主题、权限、Activity 等）。
    关键点：... 表示此处省略了其他属性（如 android:icon、android:label），实际项目中需补充。
        所有 Activity、Service、BroadcastReceiver 等组件必须嵌套在 <application> 内。

## 3. Activity 声明 <activity>

    android:name：
    指定 Activity 的完整类名（相对路径）。
    解析：
    .FirstActivity 表示 com.example.activitytest.FirstActivity（. 代表包名的相对路径）。
    错误写法：如果写成 FirstActivity（无 .），系统会尝试查找 android.FirstActivity（导致 ClassNotFoundException）。

    android:label：
    设置 Activity 在启动器（桌面图标）和任务栈中显示的标题。
    实际建议：
    通常用 字符串资源（如 @string/app_name），避免硬编码（方便多语言适配）。
    例如：android:label="@string/app_name"，并在 res/values/strings.xml 中定义。

## 4. 启动入口配置 <intent-filter>

    <action android:name="android.intent.action.MAIN">：
    表示该 Activity 是应用的主入口（系统会优先启动此 Activity）。

    <category android:name="android.intent.category.LAUNCHER">：
    表示该 Activity 显示在应用启动器（桌面图标）中。
    
    必须同时存在 MAIN 和 LAUNCHER，否则应用无法通过桌面图标启动！
    MAIN：告诉系统“这是应用的起点”。
    LAUNCHER：告诉系统“这个入口要显示在桌面”。

# 二，注册可以响应隐式Intent的activity信息  ✔
    ——————————————————————————————————————————————————————————————————————————
    <activity android:name=".SecondActivity" > 
     <intent-filter> 
     <action android:name="com.example.activitytest.ACTION_START" /> 
     <category android:name="android.intent.category.DEFAULT" /> 
     </intent-filter> 
    </activity>
    ——————————————————————————————————————————————————————————————————————————

## 解释
    在<action>标签中我们指明了当前Activity可以响应
    com.example.activitytest.ACTION_START这个action，而<category>标签则包含了
    一些附加信息，更精确地指明了当前Activity能够响应的Intent中还可能带有的category。只
    有<action>和<category>中的内容同时匹配Intent中指定的action和category时，这个
    Activity才能响应该Intent。

# 三，注册新增加的category ✔
    ————————————————————————————————————————————————————————
    <activity android:name=".SecondActivity" > 
     <intent-filter> 
     <action android:name="com.example.activitytest.ACTION_START" /> 
     <category android:name="android.intent.category.DEFAULT" /> 
     <category android:name="com.example.activitytest.MY_CATEGORY"/> 
     </intent-filter> 
    </activity>
    ————————————————————————————————————————————————————————
    错误信息提醒我们，没有任何一个Activity可以响应我们的Intent。这是因为我们刚刚在Intent
    中新增了一个category，而SecondActivity的<intent-filter>标签中并没有声明可以响
    应这个category，所以就出现了没有任何Activity可以响应该Intent的情况。现在我们在
    <intent-filter>中再添加一个category的声明。

# 四，配置一个<data>标签，用于更精确地指定当前Activity能够响应的数据。  ✔
    ————————————————————————————————————————————————————————
    <activity android:name=".ThirdActivity"> 
     <intent-filter tools:ignore="AppLinkUrlError"> 
     <action android:name="android.intent.action.VIEW" /> 
     <category android:name="android.intent.category.DEFAULT" /> 
     <data android:scheme="https" /> 
     </intent-filter> 
    </activity>
    ————————————————————————————————————————————————————————
    我们在ThirdActivity的<intent-filter>中配置了当前Activity能够响应的action是
    Intent.ACTION_VIEW的常量值，而category则毫无疑问地指定了默认的category值，另
    外在<data>标签中，我们通过android:scheme指定了数据的协议必须是https协议，这样
    ThirdActivity应该就和浏览器一样，能够响应一个打开网页的Intent了。另外，由于Android
    Studio认为所有能够响应ACTION_VIEW的Activity都应该加上BROWSABLE的category，否
    则就会给出一段警告提醒。加上BROWSABLE的category是为了实现deep link功能，和我们
    目前学习的东西无关，所以这里直接在<intent-filter>标签上使用tools:ignore属性将
    警告忽略即可。

# 五，注册对话框式主题  ✔
    ————————————————————————————————————————————————
    <activity android:name=".DialogActivity" 
     android:theme="@style/Theme.AppCompat.Dialog"> 
    </activity> 
    <activity android:name=".NormalActivity"> 
    </activity>
    ————————————————————————————————————————————————

# 六，注册Activity的启动模式 （等待项目驱动）

    —————————————————————————————————————————————————————————————————————————
    <activity 
     android:name=".FirstActivity" 
     android:launchMode="singleTop"   //(or  android:launchMode="singleTask" )
     android:label="This is FirstActivity"> 
     <intent-filter> 
     <action android:name="android.intent.action.MAIN"/> 
     <category android:name="android.intent.category.LAUNCHER"/> 
     </intent-filter> 
    </activity>
    ——————————————————————————————————————————————————————————————————————————
    <activity android:name=".SecondActivity" 
     android:launchMode="singleInstance"> 
     <intent-filter> 
     <action android:name="com.example.activitytest.ACTION_START" /> 
     <category android:name="android.intent.category.DEFAULT" /> 
     <category android:name="com.example.activitytest.MY_CATEGORY" /> 
     </intent-filter> 
    </activity> 
    ——————————————————————————————————————————————————————————————————————————

# 七，