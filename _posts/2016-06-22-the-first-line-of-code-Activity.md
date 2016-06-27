---
layout: post
title:  "《第一行代码》学习笔记之一，activity"
categories: Android
excerpt: 对第2章中提到的关于activity的知识点的总结。
tags:   android activity notes
---

* content
{:toc}

## **使用Log打印**
Android中的日志工具类是Log（android.util.Log），提供了下面几个方法来打印日志：

- `Log.v()`: 打印最为琐碎的，意义最小的日志信息。对应级别是verbose。
- `Log.d()`: 打印一些调试信息，对调试程序和分析问题应该有所帮助。
- `Log.i()`: 打印一些比较重要的数据，应该是非常想看到的，可以帮助分析用户行为的。
- `Log.w()`: 打印一些警告信息，提示程序可能会有潜在的风险，最好去修复出现警告的地方。
- `Log.e()`: 打印程序中的错误信息，一般表示程序出现了严重问题，必须尽快修复。

Log方法一般含有两个输入参数，第一个是tag，可以是任意字符串，用来过滤打印信息，一般可以设为当前类名。第二个参数是msg，就是要打印的具体内容。

```java
Log.d("test", "This is a test for log.")
```


## **隐藏活动的标题栏**
在活动的`onCreate()`方法中，添加以下代码，注意必须放在`setContentView()`调用之前。

```java
requestWindowFeature(Window.FEATURE_NO_TITLE)
```

## **消息通知--`Toast`**
`Toast`可以用于在界面上弹出一些信息通知用户，一段时间后信息会自动消失，并且不占用任何屏幕空间。

```java
Toast.makeText(MainActivity.this, "This is a Toast!", Toast.LENGTH_SHORT).show()
```

使用静态方法`makeText()`创建Toast对象，然后调用`show()`显示即可。

`makeText()`有三个输入参数，第一个是上下文，第二个是显示的文本内容，第三个是显示的时长，有两个内置常量`Toast.LENGTH_SHORT`和`Toast.LENGTH_LONG`可以使用。

## **使用菜单**

### 选项菜单（optionsMenu）
点击`menu`键时，打开的就是选项菜单。选项菜单提供了几个有用的方法，可以根据实际需要选择重载。选项菜单还可以使用自定义图标（setIcon()方法）。

- `public boolean onCreateOptionsMenu(Menu menu)`：第一次打开菜单时调用，如果返回`false`，则菜单不会被显示。
- `public boolean onOptionsItemSelected(MenuItem item)`: 选择菜单项后的响应处理。
- `public boolean onOptionsMenuClosed(Menu menu)`: 菜单关闭时的处理。
- `public boolean onPrepareOptionsMenu(Menu menu)`：菜单显示之前的处理。
- `public boolean onMenuOpened(int featureId, Menu menu)`: 菜单打开之后的处理。

使用自定义菜单，需要先在android工程的`res`目录下创建一个`menu`目录，然后在该目录下创建一个菜单资源文件。下面是一个简单的例子：

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/id_menu_add_item"
        android:title="Add" />
    <item
        android:id="@+id/id_menu_remove_item"
        android:title="Remove" />
</menu>
```

然后需要重载`onCreateOptionsMenu()`方法，这个方法的返回值表示当按下`menu`键时，该菜单是否可见。


```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    getMenuInflater().inflate(R.menu.main, menu);
    return true;
}
```

最后添加点击菜单的响应事件。先在菜单资源文件中为菜单项指定`onClick`对应的处理方法。

```xml
<item
    ...
    android:onClick="onClickMenuAddItem" />
<item
    ...
    android:onClick="onClickMenuRemoveItem" />
```

然后定义这些处理方法的实现。

```java
public void onClickMenuAddItem(MenuItem item) {
    Toast.makeText(MainActivity.this, "Add item menu is selected!", Toast.LENGTH_SHORT).show();
}

public void onClickMenuRemoveItem(MenuItem item) {
    Toast.makeText(MainActivity.this, "Remove item menu is selected!", Toast.LENGTH_SHORT).show();
}
```

另一种添加菜单响应事件的方法是重载`onOptionsItemSelected()`。这种情况下，不需要在菜单资源文件中为菜单项指定`onClick`属性。

```java
@Override
public boolean onOptionsItemSelected(MenuItem item) {
    switch (item.getItemId()) {
        case R.id.id_menu_add_item:
            Toast.makeText(this, "You clicked Add", Toast.LENGTH_SHORT).show();
            break;
        case R.id.id_menu_remove_item:
            Toast.makeText(this, "You clicked Remove", Toast.LENGTH_SHORT).show();
            break;
        default:
            break;
    }
    return true;
}
```

测试发现，当同时使用这两种方法响应菜单事件时，前一种方法（使用`onClick`属性）优先级更高。

### 上下文菜单（ContextMenu）
长按Activity界面时，打开的菜单就是上下文菜单。上下文菜单的使用方法跟选项菜单基本相同，也同样有几个方法可以按需要重载。

- `public boolean onCreateContextMenu(ContextMenu menu, View v, ContextMenuInfo menuInfo)`: 打开菜单时的调用处理。
- `public boolean onContextItemSelected(MenuItem item)`: 菜单项的响应处理。
- `public boolean onContextMenuClosed(Menu menu)`: 菜单关闭时的处理。
- `public boolean onMenuOpened(int featureId, Menu menu)`: 菜单打开之后的
- `public boolean registerForContextMenu(View view)`: 为视图注册上下文菜单。

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    requestWindowFeature(Window.FEATURE_NO_TITLE);
    setContentView(R.layout.activity_main);
    TextView tv = (TextView) findViewById(R.id.id_text_view_test);
    registerForContextMenu(tv);
}

@Override
public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
    menu.add(0, Menu.FIRST, Menu.NONE, "Context Menu 1");
    menu.add(0, Menu.FIRST+1, Menu.NONE, "Context Menu 2");
}

@Override
public boolean onContextItemSelected(MenuItem item) {
    switch (item.getItemId()) {
        case Menu.FIRST:
            Toast.makeText(this, "You clicked context menu item 1", Toast.LENGTH_SHORT).show();
            break;
        case Menu.FIRST+1:
            Toast.makeText(this, "You clicked context menu item 2", Toast.LENGTH_SHORT).show();
            break;
        default:
            break;
    }
    return true;
}
```

### 子菜单（SubMenu）
子菜单是将菜单分组进行多级显示的一种菜单。子菜单中不能再嵌套子菜单。使用子菜单的方法如下：

- 重载`onCreateOptionsMenu()`或者`onCreateContextMenu()`, 调用Menu的`addSubMenu(String title)`方法添加子菜单。
- 调用子菜单的`add(int groupId, int itemId, int order, String title)`方法，添加子菜单项。
  - groupId: 可以对菜单项进行分组，便于对一组菜单项进行统一操作。
  - itemId： 用于唯一识别菜单项，在响应菜单时需要根据itemId来判断哪一个菜单被选择。
  - order：菜单项的排列顺序，数值小的排在前面。
  - title： 菜单项显示的文本。
- 重载`onOptionsItemSelected()`或者`onContextItemSelected()`方法，响应菜单事件。

```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    SubMenu subMenuFile = menu.addSubMenu("File");
    subMenuFile.add(0, Menu.FIRST, Menu.NONE, "Open");
    subMenuFile.add(0, Menu.FIRST+1, Menu.NONE, "Save");
    return true;
}

@Override
public boolean onOptionsItemSelected(MenuItem item) {
    switch (item.getItemId()) {
        case Menu.FIRST:
            Toast.makeText(this, "You clicked File-Open", Toast.LENGTH_SHORT).show();
            break;
        case Menu.FIRST+1:
            Toast.makeText(this, "You clicked File-Save", Toast.LENGTH_SHORT).show();
            break;
        default:
            break;
    }
    return true;
}
```

## **Activity的销毁**
有两种方法：

+ 直接按Back键。
+ 在代码中调用Activity类的`finish()`方法。

## **多个Activity的切换**
通过Intent可以启动另一个Activity，并且在被启动或者返回的Activity之间传递数据。

### 手动创建新的Activity

  - 在`res`->`layout`目录下为新的activity创建layout资源文件。
  - 在`java`->`package`下为新的Activity创建一个类。

```java
public class SecondActivity extends Activity{
    ...

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.activity_second);
    }
}
```

  - 在androidManifest.xml中注册这个新的activity类。

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    ...
    <application
        ...
        <activity android:name="SecondActivity">
        </activity>
    </application>
</manifest>
```

现在可以使用新的Activity了。

### 显式Intent
这种方式显式地指定了目标活动

```java
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
startActivity(intent);
```

### 隐式Intent
这种方式并不明确指定目标Activity，只是表明目标Action和category等信息，由系统根据这些信息自动选择启动能够响应该Intent的Activity。

在Activity中通过<intent-filter>标签可以配置可以响应的intent的信息，只有intent-filter中的action和category都能匹配的时候，intent才能被该Activity响应。

intent-filter中可以有多条category，只要能匹配其中任意一条category即可。

```xml
<intent-filter>
    <action android:name="com.example.activitytest.ACTION_START" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="com.example.activitytest.MY_CATEGORY" />
</intent-filter>
```

在创建intent的时候，可以使用`addCategory()`方法来添加category。

```java
Intent intent = new Intent("com.example.activitytest.ACTION_START");
intent.addCategory("com.example.activitytest.MY_CATEGORY");
startActivity(intent);
```

隐式Intent还能够启动其它程序的活动，使得Android多个应用之间可以功能共享。

```java
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setData(Uri.parse("http://www.baidu.com"));
startActivity(intent);

Intend intent = new Intent(Intent.ACTION_DIAL);
intent.setData(Uri.parse("tel:10086"));
startActivity(intent);
```

intent-filter中可以增加data标签，更加精确的响应当前活动能响应的数据类型。只有data标签中的内容跟intent中的内容完全匹配时，当前Activity才能响应此intent。data标签主要包含：

* android:schema: 数据的协议部分，如http。
* android:host: 数据的主机名部分。
* android:port: 数据的端口部分。
* android:path: 数据的主机名和端口之后的部分，如网址中跟在域名之后的内容。
* android:mimeType: 数据类型，可以使用通配符。

### 向被启动的Activity传递数据
Intent提供了一系列`putExtra()`方法的重载，可以把想要传递的数据暂存在Intent中，另一个Activity启动之后，从Intent中取出数据即可。

发送数据示例代码

```java
String data = "hello world";
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
intent.putExtra("extra_data", data);
startActivity(intent);
```

接收数据示例代码

```java
Intent intent = getIntent();
String data = intent.getStringExtra("extra_data");
```

不同类型的数据可以调用不同的方法来获取，比如`getIntExtra()`, `getBooleanExtra()`。

### 返回数据给上一个Activity
`startActivityForResult()`也可用于启动Activity，但是期望在被启动Activity销毁时能够返回结果给上一个Activity。这个方法有两个参数，第一个是Intent，第二个是请求ID，用于在回调中判断数据来源。

调用Activity示例代码

```java
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
startActivityForResult(intent, 1);
```

被调用Activity示例代码（点击按键退出，并返回到上一个Activity）：

```java
public void onClickButtonTest() {
    Intent intent = new Intent()
    intent.putExtra("data_return", "Hello World");
    setResult(RESULT_OK, intent);
    finish();
}
```

如果通过点击Back键返回，这时为了向上一个Activity返回数据，可以重写`onBackPressed()`方法。

`setResult()`是专门用于向上一个Activity返回数据的，一般包含两个参数，第一个参数用于向上一个Activity返回处理结果，一般只有`RESULT_OK`和`RESULT_CANCELED`两个值，第二个参数是带有数据的Intent。

使用`startActivityForResult()`方法启动Activity时，在被启动的Activity被销毁之后会回调上一个Activity的`onActivityResult()`方法，所以可以重载`onActivityResult()`来获取返回的数据。

```java
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    switch (requestCode) {
    case 1:
        if (resultCode == RESULT_OK)
            String returnData = data.getStringExtra("data_return");
        break;
    default:
    }
}
```

## **Activity的生存期**
Android使用任务和栈来管理Activity。一个任务是一组放在栈中的Activity的集合，而栈是一种后入先出的数据结构。

Activity的状态可以划分为运行状态、暂停状态、停止状态和销毁状态四种，这个按照字面意思应该比较好理解。
其中暂停状态和停止状态都表示Activity已经不处于栈顶位置，如果Activity仍然可见，则为暂停状态，否则为停止状态。

根据Activity提供的回调函数，有三种生存期的定义。

* 完整生存期：在`onCreate()`和`onDestroy()`之间，一般`onCreate()`会完成各种初始化操作，`onDestroy()`完成释放内存的操作。
* 可见生存期：在`onStart()`和`onStop()`之间，在这期间，activity对用户总是可见的（也许不能进行交互）。这两个方法可以用于合理管理用户可见的资源。
* 前台生存期：在`onResume()`和`onPause()`之间，activity处于运行状态，并且可以和用户进行交互。

