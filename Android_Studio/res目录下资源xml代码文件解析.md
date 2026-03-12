# 《一》
## 一，在布局中创造Button代码
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

### 1. 根布局：<LinearLayout>

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

### 2. 子控件：<Button>
    
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

## 二，在menu文件下定义菜单布局
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
### 1，根元素 <menu>

    xmlns:android="http://schemas.android.com/apk/res/android": 这是一个命名空间声明。它告诉 Android 系统，所有以 android: 开头的属性都是由 Android 框架定义的标准属性。这是任何 Android 布局或资源文件中的常见结构。
    子元素 <item>
    这个文件中包含了两个 <item> 元素，每个元素都代表了菜单中的一个可选项。

### 2，第一个 <item>:

    android:id="@+id/add_item":
    android:id: 定义了这个菜单项的唯一标识符。
    @+id/: 这是创建新资源 ID 的语法。它表示如果名为 add_item 的 ID 在 R.java 文件中尚不存在，则创建它。
    add_item: 这是分配给这个菜单项的 ID 名称。你可以在 Java 或 Kotlin 代码中通过 R.id.add_item 来引用它。
    android:title="Add": 设置了这个菜单项在屏幕上显示的文本标题为 "Add"。

### 3，第二个 <item>:

    android:id="@+id/remove_item": 同样，为这个菜单项创建一个唯一的 ID，名称为 remove_item。
    android:title="Remove": 设置了这个菜单项的显示文本为 "Remove"。

# 《二》UI系统学习
## 一， TextView
    ————————————————————————————————————————————————
    <TextView 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:text="This is a normal activity" 
     />
    ————————————————————————————————————————————————

### 额外补充
    ————————————————————————————
    android:gravity="center"
    ————————————————————————————
    我们使用android:gravity来指定文字的对齐方式，可选值有top、bottom、start、
    end、center等，可以用“|”来同时指定多个值，这里我们指定的是"center"，效果等同
    于"center_vertical|center_horizontal"，表示文字在垂直和水平方向都居中对齐。
    
    ————————————————————————————
    android:textColor="#00ff00" 
     android:textSize="24sp"
    ————————————————————————————
    通过android:textColor属性可以指定文字的颜色，通过android:textSize属性可以指定
    文字的大小。文字大小要使用sp作为单位，这样当用户在系统中修改了文字显示尺寸时，应用程序中的文字大小也会跟着变化。、

## 二，Button
    参考以前笔记

## 三，EditText
    ——————————————————————————————————————————————————————————
    <EditText 
     android:id="@+id/editText" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     android:hint="Type something here" 
     android:maxLines="2" 
     />
    ——————————————————————————————————————————————————————————

    这里使用android:hint属性指定了一段提示性的文本

    通过android:maxLines指定了EditText的最大行数为两行，这样当输入的内容超过两行
    时，文本就会向上滚动，EditText则不会再继续拉伸

## 四，ImageView
    ——————————————————————————————————————————————————————————————
    <ImageView 
     android:id="@+id/imageView" 
     android:layout_width="wrap_content" 
     android:layout_height="wrap_content" 
     android:src="@drawable/img_1" 
     /> 
    ——————————————————————————————————————————————————————————————
    用android:src属性给ImageView指定了一张图片。由于图片的宽和高都是未知的，
    所以将ImageView的宽和高都设定为wrap_content，
    这样就保证了不管图片的尺 寸是多少，都可以完整地展示出来。

## 五，ProgressBar

### 圆形进度条

    ————————————————————————————————————————————
    <ProgressBar 
     android:id="@+id/progressBar" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     /> 
    ————————————————————————————————————————————

### 水平进度条

    —————————————————————————————————————————————
    <ProgressBar 
     android:id="@+id/progressBar" 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     style="?android:attr/progressBarStyleHorizontal" 
     android:max="100" 
     />
    —————————————————————————————————————————————
    指定成水平进度条后，我们还可以通过android:max属性给进度条设置一个最大值，

## 六，LinearLayout

    方向由 android:orientation 修改
    vertical 竖直排列
    horizontal 水平排列

    如果LinearLayout的排列方向是horizontal，内部的控件就绝对不能将宽度指定为match_parent，
    否则，单独一个控件就会将整个水平方向占满，其他的控件就没有可放置
    的位置了。同样的道理，如果LinearLayout的排列方向是vertical，
    内部的控件就不能将高度指定为match_parent。

    当LinearLayout的排列方向是horizontal时，只有垂直方向上的对齐方式才会生效。因
    为此时水平方向上的长度是不固定的，每添加一个控件，水平方向上的长度都会改变，因而无
    法指定该方向上的对齐方式。同样的道理，当LinearLayout的排列方向是vertical时，只有
    水平方向上的对齐方式才会生效。

    android:layout_weight。这个属性允许我们使用比例的方式来指定控件的大小

    为什么将android:layout_weight属性的值同时指定为1就会平分屏幕宽度呢？其实原理很
    简单，系统会先把LinearLayout下所有控件指定的layout_weight值相加，得到一个总值，
    然后每个控件所占大小的比例就是用该控件的layout_weight值除以刚才算出的总值。因此如
    果想让EditText占据屏幕宽度的3/5，Button占据屏幕宽度的2/5，只需要将EditText的
    layout_ weight改成3，Button的layout_weight改成2就可以了。

## 七，RelativeLayout
    RelativeLayout又称作相对布局，也是一种非常常用的布局。和LinearLayout的排列规则不
    同，RelativeLayout显得更加随意，它可以通过相对定位的方式让控件出现在布局的任何位 置。

## 八，FrameLayout
    FrameLayout又称作帧布局，它相比于前面两种布局就简单太多了，因此它的应用场景少了很
    多。这种布局没有丰富的定位方式，所有的控件都会默认摆放在布局的左上角。



## 九，