&emsp;&emsp;警告警告！！！你的App已经胖成猪了，再这样下去你的猪，哦不对，是你的App，你的App就没人要了。

&emsp;&emsp;随着业务越来越复杂，我们的App也越来越复杂，apk安装包也变得越来越大。安装包变得大了就会带来很多问题，最大的一个问题就是，对于大的安装包，新用户下载的意愿就变得低了，老用户也不愿意更新了。用户都不愿意下载我们的App了，那还了得，我们在后面的版本迭代中开发的功能使用的用户越来越少了，这可不行，需要立刻想办法解决。

&emsp;&emsp;磨刀不误砍柴工，在开始进行减肥瘦身之前，我们先要分析一下我们的最终的apk的组成部分有哪些，是哪些原因导致我们的App越来越胖，胖到了快被用户抛弃的程度？

#### 分析APK组成
&emsp;&emsp;在新版的Android Studio的【Build】菜单下，有一个叫做【Analysis APK】的菜单选项，点击这个选项可以从电脑上加载一个Apk文件，然后会得到如下图所示的一个分析结果：
![APK文件组成分析](http://static.liuzhenhua.net/blog.liuzhenhua.net/2017-04-09 07:29:22.568677微车APK分析.png "APK文件组成分析")
这里我使用的是我们公司的产品[【微车APP】](http://http://baike.baidu.com/link?url=zzJaoj1aoLp6jxnEvCGcKkXg5RH0LRH1PLX5O8keQNNp18frOK7LHaP4XwMAM3dEaMJ6k1Guxy_xiljs8YRC5gndbkNT692nc31jjeZxKTG "【微车】")[（微车官网）](http://weiche.me/ "（微车官网）")来作为分析apk，其实从图中可以看到，我们的app还是挺小的，只有14.3M，现在市面上的很多app动不动就是30多兆。即时这样，我还是觉得我们的app有点大了，半年前还只有10M的，这才多长时间，已经长胖50%了，必须动手了，不能放任下去了。好了，废话说了一大堆，我们还是继续分析我们的apk组成吧。我们主要关心的是“Download Size”和“% of Total Download Size”这两栏，因为在apk文件中，实际占用的存储即时这里的“Download Size”这一栏。所以从图中我们可以看出，我们微车APP中占比最大的一部分是res目录，这也是在我预料之内的，毕竟我们为了让我们微车APP具有高颜值，使用了大量精美的图片以及其他设计资源。点开res目录，发现里面主要是图片资源占大头，那么好吧，让我们先从图片着手。

#### 使用Tiny PNG压缩图片
&emsp;&emsp;在最近几个版本中，我们微车APP增加了像查询驾照分等一下车主实用功能，为了让用户更直观更快地学会使用微车的这些新功能，我们做了大量精美的用户引导功能，微车设计师们设计了很多精美的引导图片，所以导致我们的APP一下变大了。对于设计师给我们的图片，在使用之前，我们都会使用[Tiny PNG](http://www.tinypng.com "Tiny PNG")这个图片压缩网站对图片进行压缩，这个网站是一个专门压缩jpg和png格式图片的网站，压缩效果非常好，而且使用肉眼几乎看不出图片压缩前后有什么不同。同时，这个网站还提供压缩图片的API，为了方便，我们基于网站提供的API开发了我们自己专用的图片压缩脚步。好了，不给别人打广告了。即时我们的每张图片在使用前都经过了压缩，但还是不能有效阻止apk变大。

#### 使用JPG代替PNG
&emsp;&emsp;经过进一步分析apk，我们发现，我们使用的图片大多数是png格式的，我们知道png相对jpg来说，占用空间要大，所以我们考虑将一部分不带透明区域的png或者像仅仅只是因为需要一个圆角而使用png的转换成jpg，经过这一步处理，我们收获颇大，在只处理了一个分图片的情况下，apk减少了1493kb。

&emsp;&emsp;除了将部分图片从png转换成jpg，对于图片，我们还使用了多种手段来压缩图片在apk中的占比，其中应用到的有
- 在非关键性资源上只使用一套图片资源
- 适当缩小图片
- 使用着色方案

&emsp;&emsp;以上详细描述了我们apk瘦身过程中对于图片的处理过程，接下来的可能说的没有这么详细了，只大概介绍一下瘦身方法。

#### 使用一套图片资源
&emsp;&emsp;一般来说，Android为了适用不同分辨率和不同大小的屏幕，支持开发中适用多套资源文件，所以APP中可能对于一张图片的多个尺寸的副本，随着现在手机性能的提升，我们可以考虑只使用一套分辨率最高的图片。

#### 使用图片着色方案和图片变换方案
&emsp;&emsp;Android在5.0时，支持着色方案，对于一些只是颜色不同的图片资源，我们可以使用着色方案处理，而不用像以前一样，必须制作多套不同颜色的图片。5.0以下版本，对于着色功能支持较少，但是ImageView是可以支持着色方案的，所以我们还是可以适当的使用着色方案，或者图片变换的方式复用图片。
![着色方案](http://static.liuzhenhua.net/blog.liuzhenhua.net/2017-04-09 08:12:44.959471着色方案.png "着色方案")
这张图中的`ImageView`表示一个“箭头”，原本是一个灰色的箭头图片，这里使用了着色`tint`属性，使得这个箭头显示为绿色，并且使用了`rotation`让箭头旋转了90度变成向下的箭头。

#### 使用WEBP图片格式
&emsp;&emsp;对于图片资源，Google还有更耗的方案，那就是webp，webp是Google开发的一种图片压缩格式，相比传统的JPEG和png，在效率上有很大提高，废话不多说，上图：
![](http://static.liuzhenhua.net/blog.liuzhenhua.net/2017-04-09 08:38:23.907638webp对比.png)
这个Android开发官网里的一张webp和jpg图片的对比，肉眼几乎看不出什么区别，但是占用存储大小webp比jpg小很多。更多关于webp的只是，请Google。

#### 去除无用资源
&emsp;&emsp;随着APP版本的迭代，可能有些资源文件使用了又废弃了，废弃了之后可能还保留在我们的APP内没有被删除掉，Android提供了`shrinkResources`这个属性，可以在gradle打包时去除这些无用的资源文件，但我们在实施过程中发现，在gradle中指定这个属性为`true`后，并没有真的从apk中移除无用的资源，而是使用一个比原始资源更小的文件代替了无用的原始资源。基于此，我们并没有在实际应用中配置这个属性，而是选择直接删除无用的资源文件。在Android Studio中的【Refactor】菜单下有一个【Remove Unused Resources...】菜单选项，这个选项的功能可以去除工程中的无用资源文件。除了使用这个选项，我们还可以使用Android Lint中的【Unused Resources】lint功能找出无用的资源，手动删除。

#### 使用微信资源混淆工具AndResGuard
&emsp;&emsp;对于资源文件，微信开源了一个资源混淆工具【AndResGuard】，顾名思义，这个工具与Proguard类似，不过它是处理资源文件的，在打包过程中，它将资源的名字使用一些无意义的短名字，例如使用a、b、c这样的单字母代替原来的activity_main.xml这样的资源文件名，这样一来，可以显著减少apk中的resources.arsc这个文件，具体的原理我在这里就不介绍了，不明白的可以百度或者Google。【AndResGuard】工具除了这个功能之外，它还使用了7zip压缩算法生成apk，7zip比原始apk压缩效率要高，所以也能对减小apk起到一定作用。【AndResGuard】还有其他一些功能，在这里也不过多介绍。关于怎么使用，可以去github上搜索【AndResGuard】，里面有相当详细的介绍和使用示例。

#### 去除无用的语言资源文件
&emsp;&emsp;Android作为全球最流行的移动操作系统，对于国际化有非常好的支持，但是很多APP是本土化的，可能并没有国际化的需求，如果是这种情况，那么可以在gradle中配置`resConfigs`属性，去除其他语言资源文件。

#### 其他瘦身手段
还有其他一些apk瘦身手段，我们认为它们非常常见或者对瘦身效果不明显或者在具体操作上不划算，就不详细介绍了，在这里简单罗列一下：
- 开启`minifyEnable`
- 删除不必要的so文件
- 清理工程中第三方库中没有用到的功能
- 避免重复引用库
- 使用`provided`编译依赖（要视具体情况对待，大部分依赖都是运行时需要但目标平台没有的，这时不能使用`provided`）
- Facebook的`redex`字节码优化方案
- 插件化方案
- 用H5编写界面
- 从网络获取相关资源文件

这篇文章，以微车APP为例，介绍了apk瘦身方面的一些知识，作者水平有限，难免有地方了解的不够，如果读者发现文中有表述不当的地方能即时指出，那作者将感激不尽。


#### 参考文档
[Android优化系列之apk瘦身](https://yq.aliyun.com/articles/69472?spm=5176.8091938.0.0.LioCar)
[APK瘦身记，如何实现高达53%的压缩效果](https://yq.aliyun.com/articles/57284?spm=5176.8091938.0.0.hfV7Ln)
[Android资源混淆工具使用说明](https://github.com/shwenzhang/AndResGuard/blob/master/README.zh-cn.md)
[Create WebP Images](https://developer.android.com/studio/write/convert-webp.html)
[ReDex: An Android Bytecode Optimizer](https://github.com/facebook/redex)
