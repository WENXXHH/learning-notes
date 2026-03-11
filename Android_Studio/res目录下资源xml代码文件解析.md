# 一，在布局中创造Button代码
    ————————————————————————————————————————————————————————————————————————————————————————
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android" 
     android:orientation="vertical" 
     android:layout_width="match_parent" 
     android:layout_height="match_parent"> 
     
     <Button 
     android:id="@+id/button1" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="Button 1" 
     /> 
     
    </LinearLayout>
     ——————————————————————————————————————————————————————————————————————————————————————
    这是一个 最基础的 Android 布局，实现了一个 全屏按钮：
    界面效果：屏幕中央显示一个宽度占满的按钮，按钮文本为 "Button 1"。
    布局逻辑：垂直线性布局（LinearLayout）填满全屏 → 按钮宽度匹配父容器、高度自适应文本。

## 1. 根布局：<LinearLayout>

    xmlns:android：
    作用：声明 Android 的 XML 命名空间，允许使用 Android 系统提供的属性（如 android:layout_width）。
    为什么需要：如果没有此声明，Android 会无法识别 android: 前缀的属性（例如 android:orientation）。

    android:orientation="vertical"：
    作用：指定布局的排列方向为 垂直方向（子控件从上到下排列）。
    对比：若设为 horizontal，则子控件会从左到右排列。

    android:layout_width="match_parent"：
    作用：布局宽度 匹配父容器（即屏幕宽度）。
    关键点：match_parent（旧版 fill_parent）表示视图会扩展到父容器的全部宽度。

    android:layout_height="match_parent"：
    作用：布局高度 匹配父容器（即屏幕高度）。
    结果：整个 LinearLayout 会填满整个屏幕。
    ✅ 总结根布局：这是一个 垂直排列、占满全屏 的容器，用于放置内部控件。

## 2. 子控件：<Button>
    
    ndroid:id="@+id/button1"
    作用：为按钮生成唯一资源 ID（button1）。
    （@+id 与 @id 的区别：
      @+id：创建新 ID（首次使用时，系统会在 R.java 中生成该 ID）。
      @id：引用已有 ID（后续使用时用 @id/button1）。）
    
    android:layout_width="match_parent"
    作用：按钮宽度 匹配父容器（即 LinearLayout 的宽度，也就是屏幕宽度）。
    效果：按钮会横向填满整个屏幕。

    android:layout_height="wrap_content"
    作用：按钮高度 自适应内容（根据按钮文本 "Button 1" 的高度自动调整）。
    对比：若设为 match_parent，按钮高度会填满屏幕；设为 dp（如 50dp），则固定高度。

    android:text="Button 1"
    作用：设置按钮上显示的文本内容。
    关键点：文本内容决定了按钮的大小（高度由 wrap_content 决定）。
    ✅ 总结按钮：一个 宽度占满屏幕、高度自适应文本 的按钮，ID 为 button1，文本为 "Button 1"。

# 二，在menu文件下定义菜单布局
    ————————————————————————————————————————————————————————————————————————————
    <menu xmlns:android="http://schemas.android.com/apk/res/android"> 
     <item 
     android:id="@+id/add_item" 
     android:title="Add"/> 
     <item 
     android:id="@+id/remove_item" 
     android:title="Remove"/> 
    </menu> 
    ————————————————————————————————————————————————————————————————————————————
    这段 XML 代码定义了一个包含两个选项的简单菜单：
    一个名为 "Add" 的菜单项，其 ID 为 add_item。
    一个名为 "Remove" 的菜单项，其 ID 为 remove_item。
    它的作用是为你的 Android 应用提供一个标准化的、可交互的用户界面组件（菜单）。
    开发者可以通过编程方式将其加载到 Activity 中，并为 "Add" 和 "Remove" 这两个按钮分别绑定不同的功能逻辑。
## 1，根元素 <menu>

    xmlns:android="http://schemas.android.com/apk/res/android": 这是一个命名空间声明。它告诉 Android 系统，所有以 android: 开头的属性都是由 Android 框架定义的标准属性。这是任何 Android 布局或资源文件中的常见结构。
    子元素 <item>
    这个文件中包含了两个 <item> 元素，每个元素都代表了菜单中的一个可选项。

## 2，第一个 <item>:

    android:id="@+id/add_item":
    android:id: 定义了这个菜单项的唯一标识符。
    @+id/: 这是创建新资源 ID 的语法。它表示如果名为 add_item 的 ID 在 R.java 文件中尚不存在，则创建它。
    add_item: 这是分配给这个菜单项的 ID 名称。你可以在 Java 或 Kotlin 代码中通过 R.id.add_item 来引用它。
    android:title="Add": 设置了这个菜单项在屏幕上显示的文本标题为 "Add"。

## 3，第二个 <item>:

    android:id="@+id/remove_item": 同样，为这个菜单项创建一个唯一的 ID，名称为 remove_item。
    android:title="Remove": 设置了这个菜单项的显示文本为 "Remove"。

# 三， 显示文字TextView
    ————————————————————————————————————————————————
    <TextView 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="This is a normal activity" 
     />
    ————————————————————————————————————————————————

# 四，

//修改一下级别吧，以后需要添加的内容有一点多