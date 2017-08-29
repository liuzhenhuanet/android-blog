# Android调试、开发、debug技巧
Android开发是一项复杂的工作，不仅要关注界面是否在多个Android版本多个厂家的ROM下表现
正常，还得关注一些API调用在其他手机上是否得到一致的结果，因此如何快速的调试，快速的试错
就变得尤其重要。所以本文就记录一些作者平时用到的或者想要的一些辅助开发调试的工作或者操作。

### 模拟AndroidActivity和Fragment在低内存下销毁后重建
有时候我们需要测试应用中Activity在因为手机缺少内存被销毁后能否正常重建，这时候我们就需要
有一种简单的方法模拟这一过程。Android Studio用户可以在Android Monitor的工具栏选择
相应的进程后点击手机home键，让app退到后台，点击左侧*红色*小叉，然后再在手机桌面点击app
图标启动app，这时候就会发现Activity的`onCreate`方法的参数`savedInstanceState`不等
于`null`了。

上面的方法适合Android开发者，如果是测试人员呢？也非常简单，只要在终端运行`adb shell am kill 应用包名`
就可以了。

### 抓取网络请求
Facebook