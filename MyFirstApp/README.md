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

## 字符创资源
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

------> 未完待续 <------

<br/>
ikook<br/>
2017.03.13
