# Supporting Different Devices

## 支持不同的语言
在任何情况下，从应用代码中提取 UI 字符串并将其存放在外部文件中都是个好办法。Android 可以通过工程中的资源目录轻松实现这一功能。

### 创建语言区域目录和字符串文件
如需添加对更多语言的支持，在res/中创建一个额外的values目录，并以连字符和ISO国家代码结尾命名。例如，values-es/ 目录包含的简单资源用于语言代码为“es”的语言区域。Android 根据运行时设备的语言区域设置加载相应的资源。

一旦确定了为哪些语言提供支持，便可创建资源子目录和字符串资源文件。例如：

MyProject/
&emsp;&emsp;  res/
&emsp;&emsp;&emsp;&emsp;values/
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;strings.xml
&emsp;&emsp;&emsp;&emsp;values-es/
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;strings.xml
&emsp;&emsp;&emsp;&emsp;values-fr/
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;strings.xml

将各个语言区域的字符串值添加到相应文件中。

在运行时，Android 系统会根据当前为用户设备设置的语言区域使用相应的字符串资源集。

例如：英文，/values/strings.xml：
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="title">My Application</string>
    <string name="hello_world">Hello World!</string>
</resources>
```
西班牙文，/values-es/strings.xml：
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="title">Mi Aplicación</string>
    <string name="hello_world">Hola Mundo!</string>
</resources>
```
> 注：可以在任何资源类型上使用语言区域限定符（或任何配置限定符），例如，可以提供本地化版本的可绘制位图。

### 使用字符串资源
可以在源代码和其他 XML 文件中通过 < string \>元素的 name 属性来引用自己的字符串资源。

在源代码中，可以使用语法 R.string.< string_name \> 引用字符串资源。有许多方法都接受以这种方式引用的字符串资源。

例如：

```java
// Get a string resource from your app's Resources
String hello = getResources().getString(R.string.hello_world);

// Or supply a string resource to a method that requires a string
TextView textView = new TextView(this);
textView.setText(R.string.hello_world);
```

在其他 XML 文件中，只要 XML 属性接受字符串值，可以使用语法 @string/<string_name> 引用字符串资源。

例如：
```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/hello_world" />
```

## 适配不同的屏幕
Android 使用两种常规属性对不同的设备屏幕进行分类：尺寸和密度。我们应该在 APP 中包含一些替代资源，来适应不同的屏幕尺寸和密度，以优化 APP 的外观。
- 4 种普遍尺寸：小(small)，普通(normal)，大(large)，超大(xlarge)
- 4 种普遍密度：低精度(ldpi), 中精度(mdpi), 高精度(hdpi), 超高精度(xhdpi)

要为不同的屏幕声明不同的 layout 和 bitmap，必须将这些替代资源放在不同的目录中，类似于对不同语言字符串的做法

另请注意，屏幕方向（横向或纵向）被认为是屏幕尺寸的变化，因此许多应用程序应修改 layout，以优化每个方向的用户体验。

### 创建不同的 layout
要在不同的屏幕尺寸上优化用户体验，应该为要支持的每个屏幕尺寸创建一个唯一的布局 XML 文件。每个 layout 都应该保存到相应的资源目录中，并以 -< screen_size > 为后缀命名。例如，大尺寸屏幕(large screens)的唯一的 layout 文件应该保存在 res/layout-large/ 中。

> 注意: 为了匹配合适的屏幕尺寸Android会自动地测量我们的layout文件。所以不需要因不同的屏幕尺寸去担心UI元素的大小，而应该专注于layout结构对用户体验的影响。(比如关键视图相对于同级视图的尺寸或位置)

例如，此项目包括默认 layout 和大尺寸屏幕的替代布局：

MyProject/
&emsp;&emsp;  res/
&emsp;&emsp;&emsp;&emsp;layout/
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;main.xml
&emsp;&emsp;&emsp;&emsp;layout-large/
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;main.xml

layout 文件的名字必须完全一样，为了对相应的屏幕尺寸提供最优的 UI，文件的内容不同。

如平常一样在 app 中简单引用：
```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.main);
}
```
系统会根据 app 所运行的设备屏幕尺寸，在与之对应的 layout 目录中加载 layout。

>注意：Android 3.2及以上版本支持定义屏幕尺寸的高级方法，它允许我们根据屏幕最小长度和宽度，为各种屏幕尺寸指定与密度无关的layout资源。

### 创建不同的位图
应该为 4 种普遍密度(分辨率):低，中，高，超高精度，都提供相适配的 bitmap 资源。这能使 app 在所有屏幕分辨率中都能有良好的画质和效果。

要生成这些图像，应该从原始的矢量图像资源着手，然后根据下列尺寸比例，生成各种密度下的图像。
- xhdpi: 2.0
- hdpi: 1.5
- mdpi: 1.0 (基准)
- ldpi: 0.75

这意味着，如果针对 xhdpi 的设备生成了一张 200x200 的图像，那么应该为 hdpi 生成 150x150,为 mdpi 生成 100x100, 和为 ldpi 生成 75x75 的图片资源。

然后，将文件放在相应的 drawable 资源目录中：

MyProject/
&emsp;&emsp;  res/
&emsp;&emsp;&emsp;&emsp; drawable-xhdpi/
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;awesomeimage.png
&emsp;&emsp;&emsp;&emsp;drawable-hdpi/
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;awesomeimage.png
&emsp;&emsp;&emsp;&emsp;drawable-mdpi/
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;awesomeimage.png
&emsp;&emsp;&emsp;&emsp;drawable-ldpi/
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;awesomeimage.png

任何时候引用@ drawable / awesomeimage，系统都会根据屏幕的密度选择适当的 bitmap。

>注意：低密度(ldpi)资源是非必要的，当提供了hdpi的图像，系统会把hdpi的图像按比例缩小一半，去适配ldpi的屏幕。

## 支持不同的平台版本
一般情况下，在更新 app 至最新 Android 版本时，最好先保证新版的 app 可以支持 90% 的设备使用。

>注：为了能在几个 Android 版本中都能提供最好的特性和功能，应该在我们的 app 中使用 Android Support Library，它能使我们的app能在旧平台上使用最近的几个平台的APIs。

### 指定最小和目标API级别
AndroidManifest.xml 文件中描述了 app 的细节及 app 支持哪些 Android 版本。具体来说，< uses-sdk > 元素中的 minSdkVersion 和 targetSdkVersion 属性，标明在设计和测试 app 时，最低兼容 API 的级别和最高适用的 API 级别(这个最高的级别是需要通过测试的)。例如：
```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" ... >
    <uses-sdk android:minSdkVersion="4" android:targetSdkVersion="15" />
    ...
</manifest>
```
随着新版本 Android 的发布，一些风格和行为可能会改变，为了能使app能利用这些变化，而且能适配不同风格的用户的设备，应该将 targetSdkVersion 的值尽量的设置与最新可用的 Android 版本匹配。

### 运行时检查系统版本
Android 在 Build 常量类中提供了对每一个版本的唯一代号，在 app 中使用这些代号可以建立条件，保证依赖于高级别的 API 的代码，只会在这些 API 在当前系统中可用时，才会执行。
```java
private void setUpActionBar() {
    // Make sure we're running on Honeycomb or higher to use ActionBar APIs
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.HONEYCOMB) {
        ActionBar actionBar = getActionBar();
        actionBar.setDisplayHomeAsUpEnabled(true);
    }
}
```
>注意：当解析 XML 资源时，Android 会忽略当前设备不支持的 XML 属性。所以可以安全地使用较新版本的 XML 属性，而不需要担心旧版本 Android 遇到这些代码时会崩溃。例如如果设置 targetSdkVersion="11"，app 会在 Android 3.0 或更高时默认包含 ActionBar。然后添加 menu items到 action bar 时，需要在自己的 menu XML 资源中设置 android:showAsAction="ifRoom"。在跨版本的 XML 文件中这么做是安全的，因为旧版本的 Android 会简单地忽略 showAsAction 属性(就是这样，你并不需要用到 res/menu-v11/ 中单独版本的文件)。

### 使用平台样式和主题
Android 提供了用户体验主题，为 app 提供基础操作系统的外观和体验。这些主题可以在 manifest 文件中被应用于 app 中。通过使用内置的风格和主题，app 自然地随着 Android 新版本的发布，自动适配最新的外观和体验。

使 activity 看起来像对话框：
```xml
<activity android:theme="@android:style/Theme.Dialog">
```

使 activity 有一个透明背景：
```xml
<activity android:theme="@android:style/Theme.Translucent">
```

应用在 /res/values/styles.xml 中定义的自定义主题：
```xml
<activity android:theme="@style/CustomTheme">
```

使整个app应用一个主题(全部 activities )在元素中添加 android:theme 属性：
```xml
<application android:theme="@style/CustomTheme">
```

<br/>
ikook<br/>2017.03.16
