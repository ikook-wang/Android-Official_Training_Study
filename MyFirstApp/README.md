# <Center/> Training First App </center>


## View
Android 应用的图形界面使用 View 对象以及 ViewGroup对象层次结构而构建。View 对象通常为按钮或文本字段之类的 UI 小部件。而 ViewGroup 对象则为不可见的视图容器，它们定义子视图的布局。

![ViewGroup 对象如何在布局中形成分支并容纳其他 View 对象的图解](http://oe94sy64u.bkt.clouddn.com/viewgroup.png)

## 替代布局
以 XML 格式（而不是运行时代码的方式）声明 UI 布局有若干用处，但其中最重要的用处便是，可以创建不同的布局来适应不同的屏幕尺寸。 例如，可以创建两个版本的布局，并指示系统在“小”屏幕上使用哪个版本，在“大”屏幕上使用哪个版本。

## LinearLayout
LinearLayout 是一个视图组(ViewGroup的子类)，它会按照 android:orientation 属性的指定，将子视图设置为垂直或水平方向布局。LinearLayout 的每个子视图都会按照它们各自在 XML 中的出现顺序显示在屏幕上。

如果 LinearLayout 是布局中的根视图，则应将宽度和高度设置为 "match_parent"，从而填满可供应用使用的整个屏幕区域。 该值表示视图应扩大其宽度或高度，以匹配父视图的宽度或高度。

## 布局相关属性
- #### android:id
这会为视图赋予唯一的标识符，可以使用该标识符从应用代码中引用对象，例如读取和操作对象。
从 XML 引用任何资源对象时，都需要使用 @ 符号。 请在该符号后依次输入资源类型、斜杠和资源名称：android:id="@+id/edit_message(例子)。

  只有在第一次定义资源 ID 时，才需要在资源类型之前加一个加号 (+)。 当编译应用时，SDK 工具会使用 ID 名称在项目的 R.java 文件中新建一个引用 EditText(例子) 元素的资源 ID。一旦以此方式声明资源 ID，其他对该 ID 的引用皆无需使用加号。 只有在指定新资源 ID 时才必须使用加号，对于字符串或布局等具体资源则不必如此。
- #### android:layout_width 和 android:layout_height
"wrap_content" 值并不规定宽度和高度的具体大小，而是指定根据需要缩放视图，使其适合视图的内容。 如果要改用 "match_parent"，则 EditText 元素将填满屏幕，因为它会匹配父 LinearLayout(例子) 的大小。

- #### android:hint
这是文本字段为空时显示的默认字符串。"@string/edit_message"(例子) 并非使用硬编码字符串作为其值，而是引用另一个文件中定义的一个字符串资源。 由于它引用的是一个具体资源（而不仅仅是标识符），因此不需要加号。

- #### android:layout_weight
weight 值是一个数字，用于指定每个视图与其他同级视图在剩余空间中的占比。 这有点像饮料配方中各种成分的比例： “2 份苏打、1 份糖浆”是指饮料中三分之二是苏打。例如，如果将一个视图的 weight 值指定为 2，将另一个视图的 weight 值指定为 1，总和是 3，那么第一个视图将填满剩余空间的 2/3，而第二个视图则填满其余部分。 如果添加了第三个视图，将其 weight 值指定为 1，那么现在第一个视图（weight 值为 2）将获得 1/2 的剩余空间，其余两个视图则各占 1/4。

  所有视图的默认 weight 值都为 0，所以如果仅将一个视图的 weight 值指定为大于 0，那么等到其他所有视图都获得所需空间后，该视图便会填满所有剩余空间。

  优化: 如需使其一个部件的宽度或者高度占满剩余屏幕空间时，例如在宽度方向占满屏幕剩余空间。设置 android:layout_weight="1" 属性后，将宽度设置为零 (0dp) 可提高布局性能，这是因为如果将宽度设置为 "wrap_content"或其他值，则会要求系统计算宽度，而该计算最终毫无意义，因为 weight 值还需要计算另一个宽度，才能填满剩余空间。

> 注：对资源的引用始终都按资源类型（如 id 或 string）确定其作用域，因此使用相同的名称不会引起冲突

## 资源对象
资源对象是一个唯一的整型名称，它与应用资源（如位图、布局文件或字符串）相关联。

每个资源在项目的 R.java 文件中都定义有相应的资源对象。可以使用 R 类中的对象名称来引用资源，例如当需要为 android:hint 属性指定字符串值时，就可以这样做。也可以使用 android:id 属性创建任意与视图相关联的资源 ID，从而可以从其他代码中引用该视图。

SDK 工具会在您每次编译应用时生成 R.java 文件。切勿手动修改该文件。

## 字符串资源
对于用户界面中的文本，务必将每个字符串都指定为资源。 字符串资源允许在单一位置管理所有 UI 文本，从而简化文本的查找和更新。 此外，将字符串外部化还可为每个字符串资源提供替代定义，从而将应用本地化为不同的语言。

## 响应按钮
可在布局文件的 Button 元素中添加 android:onClick 属性，如 android:onClick="sendMessage"。每次用户点击按钮时，此属性均会提示系统调用 Activity 中的 sendMessage() 方法。

还需在布局文件对应的 Activity 类中添加与 onClick 属性对应的方法。如：
```java
/**Called when the user clicks the Send button */
public void sendMessage(View view) {
    // Do something in response to button
}
```
要让系统将此方法与为 android:onClick 指定的方法名称匹配，签名必须与所示内容完全相同。具体而言，该方法必须：
- 是公共方法
- 具有空返回值
- 以 View 作为唯一参数（这将是之前点击的 View）

## Intent
Intent 是指在相互独立的组件（如两个 Activity）之间提供运行时绑定功能的对象。Intent 表示一个应用“执行某项操作的 Intent”。 可以将 Intent 用于各种任务。例如：Intent 用于启动另一个 Activity。
```java
   /** Called when the user clicks the Send button */
   public void sendMessage(View view) {
       Intent intent = new Intent(this, DisplayMessageActivity.class);
       EditText editText = (EditText) findViewById(R.id.edit_message);
       String message = editText.getText().toString();
       intent.putExtra(EXTRA_MESSAGE, message);
       startActivity(intent);
   }
```
Intent 构造函数采用两个参数：<br/>
Context 是第一个参数（之所以使用 this ，是因为 Activity 类是 Context 的子类）
应用组件的 Class，系统应将 Intent（在本例中，为应启动的 Activity）传递至该类。

putExtra() 方法将 EditText 的值添加到 Intent。Intent 能够以名为 extra 的键值对形式携带数据类型。键是一个公共常量 EXTRA_MESSAGE，因为下一个 Activity 将使用该键来检索文本值。为 Intent extra 定义键时最好使用应用的软件包名称作为前缀。这可以确保在应用与其他应用交互过程中这些键始终保持唯一。

startActivity() 方法将启动 Intent 指定的 DisplayMessageActivity 实例。

编写 DisplayMessageActivity.java 类，用于显示第一个 Activity 传递的消息。代码如下
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
   super.onCreate(savedInstanceState);
   setContentView(R.layout.activity_display_message);

   Intent intent = getIntent();
   String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE);
   TextView textView = new TextView(this);
   textView.setTextSize(40);
   textView.setText(message);

   ViewGroup layout = (ViewGroup) findViewById(R.id.activity_display_message);
   layout.addView(textView);
}
```

此处会执行大量操作，因此我们接着解释：

1. 调用 getIntent() 采集启动 Activity 的 intent。无论用户如何导航到目的地，每个 Activity 都由一个 Intent 调用。 调用 getStringExtra() 将检索第一个 Activity 中的数据。

2. 可以使用编程方式创建 TextView 并设置其大小和消息。

3. 可将 TextView 添加到 R.id.activity_display_message 标识的布局。可将布局投射到 ViewGroup，因为它是所有布局的超类且包含 addView() 方法。

## 布局
布局定义用户界面的视觉结构，如Activity或应用小部件的 UI。可以通过两种方式声明布局：
- 在 XML 中声明 UI 元素。Android 提供了对应于 View 类及其子类的简明 XML 词汇，如用于小部件和布局的词汇；
- 运行时实例化布局元素。应用可以通过编程创建 View 对象和 ViewGroup 对象（并操纵其属性）。

Android 框架可以灵活地使用以下一种或两种方法来声明和管理应用的 UI。例如，可以在 XML 中声明应用的默认布局，包括将出现在布局中的屏幕元素及其属性。然后，可以在应用中添加可在运行时修改屏幕对象（包括那些已在 XML 中声明的对象）状态的代码。

在 XML 中声明 UI 的优点在于，可以更好地将应用的外观与控制应用行为的代码隔离。UI 描述位于应用代码外部，这意味着在修改或调整描述时无需修改源代码并重新编译。例如，可以创建适用于不同屏幕方向、不同设备屏幕尺寸和不同语言的 XML 布局。此外，在 XML 中声明布局还能更轻松地显示 UI 的结构，从而简化问题调试过程。

一般而言，用于声明 UI 元素的 XML 词汇严格遵循类和方法的结构和命名方式，其中元素名称对应于类名称，属性名称对应于方法。实际上，这种对应关系往往非常直接，可以猜到对应于类方法的 XML 属性，或对应于给定 XML 元素的类。但请注意，并非所有词汇都完全相同。在某些情况下，在命名上略有差异。例如，EditText 元素具有的 text 属性对应的类方法是 EditText.setText()。

### 编写 XML
可以利用 Android 的 XML 词汇，按照在 HTML 中创建包含一系列嵌套元素的网页的相同方式快速设计 UI 布局及其包含的屏幕元素。

每个布局文件都必须只包含一个根元素，并且该元素必须是视图对象或 ViewGroup 对象。定义根元素之后，即可再以子元素的形式添加其他布局对象或小部件，从而逐步构建定义布局的视图层次结构。例如，以下这个 XML 布局使用垂直 LinearLayout 来储存一个 TextView 和一个 Button：
```XML
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:orientation="vertical" >
    <TextView android:id="@+id/text"
              android:layout_width="wrap_content"
              android:layout_height="wrap_content"
              android:text="Hello, I am a TextView" />
    <Button android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Hello, I am a Button" />
</LinearLayout>
```
在 XML 中声明布局后，请在 Android 项目 res/layout/ 目录中以 .xml 扩展名保存文件，以便其能够正确编译。

### 加载 XML 资源
当编译应用时，每个 XML 布局文件都会编译到一个 View 资源中。 应该在 Activity.onCreate() 回调实现中从应用代码加载布局资源。通过调用 setContentView()，以 R.layout.layout_file_name 形式向其传递对布局资源的引用来执行此操作。例如，如果 XML 布局保存为 main_layout.xml，则需要像下面这样为 Activity 加载该布局：
```java
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.main_layout);
}
```
启动 Activity 时，Android 框架会调用 Activity 中的 onCreate() 回调方法

### 属性
每个视图对象和 ViewGroup 对象都支持各自的各类 XML 属性。某些属性是视图对象的专用属性（例如，TextView 支持 textSize 属性），但这些属性也会被任何可以扩展此类的视图对象继承。某些属性通用于所有 View 对象，因为它们继承自根 View 类（如 id 属性）。此外，其他属性被视为“布局参数”，即描述 View 对象特定布局方向的属性，如该对象的父 ViewGroup 对象所定义的属性。

- #### ID
任何视图对象都可能具有关联的整型 ID，此 ID 用于在结构树中对 View 对象进行唯一标识。编译应用后，此 ID 将作为整型数引用，但在布局 XML 文件中，通常会在 id 属性中为该 ID 赋予字符串值。这是所有 View 对象共用的 XML 属性（由 View 类定义），经常会用到它。XML 标记内部的 ID 语法是：
```xml
android:id="@+id/my_button"
```
字符串开头处的 @ 符号指示 XML 解析程序应该解析并展开 ID 字符串的其余部分，并将其标识为 ID 资源。加号 (+) 表示这是一个新的资源名称，必须创建该名称并将其添加到我们的资源（在 R.java 文件中）内。Android 框架还提供了许多其他 ID 资源。 引用 Android 资源 ID 时，不需要加号，但必须添加 android 软件包命名空间，如下所示：
```xml
android:id="@android:id/empty"
```
添加 android 软件包命名空间之后，现在，将从 android.R 资源类而非本地资源类引用 ID。

 要想创建视图并从应用中引用它们，常见的模式是：

 1. 在布局文件中定义一个视图/小部件，并为其分配一个唯一的 ID：
```xml
<Button android:id="@+id/my_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/my_button_text"/>
```
 2. 然后创建一个 view 对象实例，并从布局中捕获它（通常使用 onCreate() 方法）：
 ```java
 Button myButton = (Button)findViewById(R.id.my_button);
 ```

 创建 RelativeLayout 时，为 view 对象定义 ID 非常重要。在相对布局中，同级视图可以定义其相对于其他同级视图的布局，同级视图通过唯一的 ID 进行引用。

 ID 不需要在整个结构树中具有唯一性，但在要搜索的结构树部分应具有唯一性（要搜索的部分往往是整个结构树，因此最好尽可能具有全局唯一性）。

- #### 布局参数
名为 layout_something 的 XML 布局属性可为视图定义与其所在的 ViewGroup 相适的布局参数。

 每个 ViewGroup 类都会实现一个扩展 ViewGroup.LayoutParams 的嵌套类。此子类包含的属性类型会根据需要为视图组的每个子视图定义尺寸和位置。 正如在下图中所见，父视图组为每个子视图（包括子视图组）定义布局参数。

  ![以可视化方式表示的视图层次结构，其中包含与每个视图关联的布局参数](http://oe94sy64u.bkt.clouddn.com/layoutparams.png)

 请注意，每个 LayoutParams 子类都有自己的值设置语法。 每个子元素都必须定义适合其父元素的 LayoutParams，但父元素也可为其子元素定义不同的 LayoutParams。

 所有视图组都包括宽度和高度（layout_width 和 layout_height），并且每个视图都必须定义它们。许多 LayoutParams 还包括可选的外边距和边框。

 可以指定具有确切尺寸的宽度和高度，但多半不想经常这样做。 在更多的情况下，会使用以下常量之一来设置宽度或高度：

  - wrap_content 指示您的视图将其大小调整为内容所需的尺寸。

  - match_parent 指示您的视图尽可能采用其父视图组所允许的最大尺寸。<br/>

 一般而言，建议不要使用绝对单位（如像素）来指定布局宽度和高度， 而是使用相对测量单位，如密度无关像素单位 (dp)、wrap_content 或 match_parent，这种方法更好，因为它有助于确保应用在各类尺寸的设备屏幕上正确显示。

### 布局位置
视图的几何形状就是矩形的几何形状。视图具有一个位置（以一对水平向左和垂直向上坐标表示）和两个尺寸（以宽度和高度表示）。 位置和尺寸的单位是像素。

 可以通过调用方法 getLeft() 和方法 getTop() 来检索视图的位置。前者会返回表示视图的矩形的水平向左（或称 X 轴） 坐标。后者会返回表示视图的矩形的垂直向上（或称 Y 轴）坐标。 这些方法都会返回视图相对于其父项的位置。 例如，如果 getLeft() 返回 20，则意味着视图位于其直接父项左边缘向右 20 个像素处。

 此外，系统还提供了几种便捷方法来避免不必要的计算，即 getRight() 和 getBottom()。 这些方法会返回表示视图的矩形的右边缘和下边缘的坐标。 例如，调用 getRight() 类似于进行以下计算：getLeft() + getWidth()。

### 尺寸、内边距和外边距
视图的尺寸通过宽度和高度表示。视图实际上具有两对宽度和高度值。

 第一对称为测量宽度和测量高度。 这些尺寸定义视图想要在其父项内具有的大小。 这些测量尺寸可以通过调用 getMeasuredWidth() 和 getMeasuredHeight() 来获得。

 第二对简称为宽度和高度，有时称为绘制宽度和绘制高度。 这些尺寸定义视图在绘制时和布局后在屏幕上的实际尺寸。 这些值可以（但不必）与测量宽度和测量高度不同。 宽度和高度可以通过调用 getWidth() 和 getHeight() 来获得。

 要想测量其尺寸，视图需要将其内边距考虑在内。内边距以视图左侧、顶部、右侧和底部各部分的像素数表示。 内边距可用于以特定数量的像素弥补视图的内容。 例如，左侧内边距为 2，会将视图的内容从左边缘向右推 2 个像素。 可以使用 setPadding(int, int, int, int) 方法设置内边距，并通过调用 getPaddingLeft()、getPaddingTop()、getPaddingRight() 和 getPaddingBottom() 进行查询。

 尽管视图可以定义内边距，但它并不支持外边距。 不过，视图组可以提供此类支持。

### 常见布局
ViewGroup 类的每个子类都提供了一种独特的方式来显示在其中嵌套的视图。以下是 Android 平台中内置的一些较为常见的布局类型。

 > 注：尽管可以通过将一个或多个布局嵌套在另一个布局内来实现您的 UI 设计，但应该使您的布局层次结构尽可能简略。布局的嵌套布局越少，绘制速度越快（扁平的视图层次结构优于深层的视图层次结构）。

 **线性布局：** 一种使用单个水平行或垂直行来组织子项的布局。它会在窗口长度超出屏幕长度时创建一个滚动条。

   ![](http://oe94sy64u.bkt.clouddn.com/linearlayout-small.png)

 **相对布局：** 能够指定子对象彼此之间的相对位置（子对象 A 在子对象 B 左侧）或子对象与父对象的相对位置（与父对象顶部对齐）。

  ![](http://oe94sy64u.bkt.clouddn.com/relativelayout-small.png)

 **网页视图：** 显示网页。

  ![](http://oe94sy64u.bkt.clouddn.com/webview-small.png)

### 使用适配器构建布局
如果布局的内容是属于动态或未预先确定的内容，您可以使用这样一种布局：在运行时通过子类 AdapterView 用视图填充布局。 AdapterView 类的子类使用 Adapter 将数据与其布局绑定。Adapter 充当数据源与 AdapterView 布局之间的中间人—Adapter（从数组或数据库查询等来源）检索数据，并将每个条目转换为可以添加到 AdapterView 布局中的视图。

 适配器支持的常见布局包括：

 **列表视图：** 显示滚动的单列列表。

 ![](http://oe94sy64u.bkt.clouddn.com/listview-small.png)

 **网格视图：** 显示滚动的行列网格。

 ![](http://oe94sy64u.bkt.clouddn.com/gridview-small.png)

 - #### 使用数据填充适配器视图
可以通过将 AdapterView 实例与 Adapter 绑定来填充 AdapterView（如 ListView 或 GridView），此操作会从外部来源检索数据，并创建表示每个数据条目的 View。

 Android 提供了几个 Adapter 子类，用于检索不同种类的数据和构建 AdapterView 的视图。 两种最常见的适配器是：

  - ##### ArrayAdpater:

    请在数据源为数组时使用此适配器。默认情况下，ArrayAdapter 会通过在每个项目上调用 toString() 并将内容放入 TextView 来为每个数组项创建视图。

    例如，如果具有想要在 ListView 中显示的字符串数组，请使用构造函数初始化一个新的 ArrayAdapter，为每个字符串和字符串数组指定布局：
   ```java
 ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
        android.R.layout.simple_list_item_1, myStringArray);
 ```
   此构造函数的参数是：
    - 应用 Context

    - 包含数组中每个字符串的 TextView 的布局

    - 字符串数组

    然后，只需在 ListView 上调用 setAdapter()：
    ```java
 ListView listView = (ListView) findViewById(R.id.listview);
listView.setAdapter(adapter);
 ```
   要想自定义每个项的外观，您可以重写数组中各个对象的 toString() 方法。或者，要想为 TextView 之外的每个项创建视图（例如，如果想为每个数组项创建一个 ImageView），请扩展 ArrayAdapter 类并重写 getView() 以返回想要为每个项获取的视图类型。

  - ##### SimpleCursorAdapter:

    请在数据来自 Cursor 时使用此适配器。使用 SimpleCursorAdapter 时，必须指定要为 Cursor 中的每个行使用的布局，以及应该在哪些布局视图中插入 Cursor 中的哪些列。 例如，如果想创建人员姓名和电话号码列表，则可以执行一个返回 Cursor（包含对应每个人的行，以及对应姓名和号码的列）的查询。 然后，可以创建一个字符串数组，指定想要在每个结果的布局中包含 Cursor 中的哪些列，并创建一个整型数组，指定应该将每个列放入的对应视图：
 ```java
 String[] fromColumns = {ContactsContract.Data.DISPLAY_NAME,
                        ContactsContract.CommonDataKinds.Phone.NUMBER};
int[] toViews = {R.id.display_name, R.id.phone_number};
 ```
   当实例化 SimpleCursorAdapter 时，请传递要用于每个结果的布局、包含结果的 Cursor 以及以下两个数组：
 ```java
 SimpleCursorAdapter adapter = new SimpleCursorAdapter(this,
        R.layout.person_name_and_number, cursor, fromColumns, toViews, 0);
ListView listView = getListView();
listView.setAdapter(adapter);
 ```
   然后，SimpleCursorAdapter 会使用提供的布局，将每个 fromColumns 项插入对应的 toViews 视图，为 Cursor 中的每个行创建一个视图。

   如果在应用的生命周期中更改了适配器读取的底层数据，则应调用 notifyDataSetChanged()。此操作会通知附加的视图，数据发生了变化，它应该自行刷新。

 - #### 处理点击事件
 可以通过实现 AdapterView.OnItemClickListener 界面来响应 AdapterView 中每一项上的点击事件。 例如：
 ```java
 // 创建一个匿名类作为消息处理对象。
 private OnItemClickListener mMessageClickedHandler = new OnItemClickListener() {
     public void onItemClick(AdapterView parent, View v, int position, long id) {
         // Do something in response to the click
     }
 };
 listView.setOnItemClickListener(mMessageClickedHandler);
 ```

<br/>
ikook<br/>
2017.03.13
