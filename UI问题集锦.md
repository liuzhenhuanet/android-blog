## Android UI问题集锦
这篇博客用来记录作者在Android开发过程中遇到的一些UI上的问题，本博客持续更新，欢迎关注。
作者：刘振华  邮箱：liuzhenhuanet@gmail.com  个人网站：blog.liuzhenhua.net  github：github.com/liuzhenhuanet

### View构造方法参数分别代表什么，都有什么作用？
`View`有四个构造方法，分别是：
```java
public View(Context context)
public View(Context context, @Nullable AttributeSet attrs)
public View(Context context, @Nullable AttributeSet attrs, int defStyleAttr)
public View(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes)
```
这么几个构造方法其实可以理解为一个构造方法，前面三个方法直接或间接地调用最后一个构造方法，并传入默认参数。
那么我们就只分析最后一个构造方法，这个构造方法是在API 21加入的。
第一个参数`Context`大家都很熟悉了，这个参数一般来说应该传入一个`Activity`，但也可以是`Application`等其他`Context`子类，
但是如果传入的不是`Activity`并且需要在View内部使用`Context`启动一个`Activity`，则必须记住给`Intent`增加参数
`Intent.FLAG_ACTIVITY_NEW_TASK`，不然启动时会崩溃。另外`Context`还决定了页面的主题，所以同样的View，通过不同的
`Context`构造，显示样式可能差别非常大。
第二个参数`AttributeSet`，可以为`null`，这个参数是一个属性集，一般在`xml`中设置的属性会通过这个参数传入。
第三个参数`defStyleAtrr`，是一个`R.attr.xxx`属性，从`Context`的主题中获取属性值对应的样式。例如主题中定义了一个item
属性：`<item name="myStyleAttr">@style/beautifulStyle</item>`，`defStyleAttr`参数传入的值是`R.attr.myStyleAttr`，
则当`AttributeSet`参数没有设置相关属性集时则从`beautifulStyle`中获取。
第四个参数`defStyleRes`与第三个参数类似，当前面的参数都没有设置相关的属性时，则会从第四个参数里面获取，这个参数指的是一个
Style资源。

### 如何通过`Java`代码设置`EditText`显示滚动条？
如果`EditText`中的内容超过了View内容高度，想要显示滚动条该怎么做呢？我们知道可以在布局文件中设置属性：
`android:scrollbars="vertical"`，这样就可以让其在竖直方向显示滚动条了。但是如果`EditText`是在Java代码中直接
`new`出来的呢？网上找到的答案基本都是在`Java`代码中调用`View.setVerticalScrollBarEnabled(true)`方法，我试了一下，
是无效的。我跟踪到`View`类的构造方法的代码里找到下面这段代码：
```java
if (initializeScrollbars) {
    initializeScrollbarsInternal(a);
}
```
`initializeScrollbarsInternal(a);`方法主要是初始化`scrollbar`相关样式的，`initializeScrollbars`的值是`View`的
`R.styleable.View_scrollbars`样式属性值，所以不论我是在xml定义`EditText`还是直接`new EditText`后再调用
`View.setVerticalScrollBarEnabled(true)`方法都不能显示滚动条，因为除了在构造方法中能够调用`initializeScrollbarsInternal(a);`
方法之外，没有其他地方调用这个方法了，因此也就没有设置`scrollbar`样式，从而导致不能显示滚动条。  
那么如果我是用java代码直接new的`EditText`要怎样才能显示`scrollbar`呢？答案就是通过传入合适的构造方法参数，使
`initializeScrollbars`为true，`attrs`是xml设置的属性，我们不是从xml构造的控件，所以这个参数不行。`defStyleRes`
参数是API 21才加入的，为了兼容老版本这个参数也不行，那只剩下`defStyleAttr`这个参数了，这个参数是通过指定采用主题中的哪个
属性值对应的样式（style）来设置`View`属性，而跟踪到`EditText`构造方法：
```java
public EditText(Context context, AttributeSet attrs) {
    this(context, attrs, com.android.internal.R.attr.editTextStyle);
}
```
发现默认是使用的`com.android.internal.R.attr.editTextStyle`，所以我们只要在`EditText`使用的主题中增加如下代码：
```xml
<item name="android:editTextStyle">@style/myStyle</item>
```
其中`myStyle`样式中设置至少一个item：
```xml
<item name="android:scrollbars">vertical</item>
```
那么，一切都搞定了。

### 下一个话题，敬请期待……