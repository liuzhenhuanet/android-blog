# Android Studio实用技巧2：使用Android Studio分析你的APK以及查看APK的一些关键信息 

本文要解决的问题：

- 使用Android Studio分析APK文件、查看APK里面dex文件、资源文件、AndroidManifest等文件内容
- 使用Android Studio对比不同版本APK文件差异

### 写在前面的废话
我使用的是Android Studio2.3.1，记不清楚是在哪个版本开始有这个功能的，但在2.3.1之前的几个版本肯定支持的，其他版本的支持情况请读者自行查看。本文采用了大量的图片来描述，保证简单易操作，建议读者亲自动手实践一下，这样才能有更深的体会。另外，为了保持本文篇幅短、操作简单，同时鉴于作者的懒惰，所以每一个步骤只是简单截了个图，没有更深入到每个细节步骤。  

### 进入正题
1. 第一步需要在Android Studio上找到这个功能的入口，为了描述方便，我特地截了图，并加上了简单指引，如下图：  
![APK分析入口](https://raw.githubusercontent.com/liuzhenhuanet/android-blog/master/images/APK分析入口.png "APK分析入口")  
打开Android Studio后，按下顶部菜单栏上的【Build】，然后选择【Analyze APK】，这时会弹出选择APK文件选择对话框，选择你需要分析的APK即可。  

2. 选择了APK后会看到如下图，列出了APK压缩文件内的所有目录和文件信息：  
![Android Studio打开apk](https://raw.githubusercontent.com/liuzhenhuanet/android-blog/master/images/Android_Studio打开apk.png "Android Studio打开apk")  

3. 这跟我们直接解压看到的apk文件是一样的，但是在这里，我们可以点击其中任何文件，Android Studio将会显示文件里的内容，例如我点击`resources.arsc`文件，看到下图所示，会在底部显示所有的资源名称以及资源值，并且资源值是按类型划分的，所以想在APK中找一个资源值是很方便的：  
![查看APK中的资源值](https://raw.githubusercontent.com/liuzhenhuanet/android-blog/master/images/查看APK中的资源值.png "查看APK中的资源值")  
我们查看一下dex文件，如下图：  
![查看APK中的dex](https://raw.githubusercontent.com/liuzhenhuanet/android-blog/master/images/查看APK中的dex文件类结构.png "查看APK中的dex文件类结构")   
可以看到我们看到整个Java包结构，甚至可以看到一个类都有哪些方法，但是可惜的是，无法查看类反编译信息，但是我们也不能要求太多，如果想查看反编译后的代码，可以使用专业的反编译工具，例如我经常使用的apktool、jd-gui和jadx，在这里我强烈推荐使用jadx，因为它夸平台，并且提供了一条龙服务，Java和资源的反编译结果都能看到，如果你的代码没有混淆，那你看到的就像看的项目工程一样，关于jadx的更多信息，在本文里就不多说了，读者可以自行Google。  
   
4. 最后一点了，是我觉得非常有用的一点，那就是比较不同APK（最好是不同版本的同一个APP），如下图，你可以看到不同APK文件中哪些文件相同，哪些文件不同，并且可以详细看到文件变大了多少、变小了多少，这样我们就可以知道APK包变大了到底是哪一部分变大了，这样我们就可以有针对性的采用APK瘦身方案，关于瘦身方案，读者可以查看我之前写的《Android App瘦身实践：让你的App瘦成一道闪电》，不说废话了，还是先把图放上来吧：  
![比较不同APK之间的差异](https://raw.githubusercontent.com/liuzhenhuanet/android-blog/master/images/比较不同APK之间的差异.png "比较不同APK之间的差异")    

