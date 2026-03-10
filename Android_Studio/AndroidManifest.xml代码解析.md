# 一，注册Activity,声明主activity
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

# 二，
