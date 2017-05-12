### 本文的目标
- 简单介绍APK安装过程中涉及到的目录、文件
- 简单介绍整个安装过程中系统进行了哪些操作
- 以及安装过程中牵涉到的其他概念

### APK安装方式
- 系统应用安装——首次开机时完成，没有安装界面
- 网络下载安装——通过应用市场安装，没有安装界面
- ADB安装——没有安装界面
- 本地安装——通过点击SD卡里的APK文件安装，有安装界面，由packageinstaller.apk应用处理安装及卸载过程
注：以上说的显示不显示安装界面只针对原生系统。  

### 安装过程中涉及到的目录和文件
- system/app/&emsp;&emsp;系统应用apk文件存放地
- data/app/&emsp;&emsp;用户程序apk文件存放地 
- data/data/&emsp;&emsp;存放应用程序数据
- data/dalvik-cache&emsp;&emsp;dex文件缓存目录，用户app安装后会解压出apk文件里的dex文件，解压后的dex缓存在这个目录下应用对应包名的子文件夹下
- data/system/packages.list&emsp;&emsp;这个文件里存放的是所有已安装APP的列表，有四列，依次是：packageName,userId,debugFlag,dataPath(应用对应的数据目录)，例如：`com.android.camera 10025 0 /data/data/com.android.camera`
- data/system/packages.xml&emsp;&emsp;这个文件记录已安装的app的包名，安装包位置，nativeLibraryPath，还有uid，签名证书，所需权限等信息，相当于packages.list文件的复杂版，信息更全面

### odex文件组成格式
![odex文件结构](https://raw.githubusercontent.com/liuzhenhuanet/android-blog/master/images/odex文件结构.png "odex文件结构")  

### 安装过程
- 等待；
- 添加一个包文件到安装进程的队列中；
- 确定合适的地方来安装包文件；
- 复制apk文件到一个给定的目录下；
- 确定应用的UID；
- 请求installd守护程序进程；
- 创建应用目录和设置权限；
- 提取dex代码到缓存目录中；
- 解析packages.list、system、data和packages.xml的最新状态；
- 向系统发送广播消息，消息带有安装完成效果的名字Intent.ACTION_PACKAGE_ADDED：如果是更新，会带有新的（Intent.ACTION_PACKAGE_REPLACED）。 

#### 参考文献
[https://wenku.baidu.com/view/69da4ae919e8b8f67c1cb95a.html](https://wenku.baidu.com/view/69da4ae919e8b8f67c1cb95a.html "Android 包管理(APK管理)机制详细解析")&emsp;&emsp;Android 包管理(APK管理)机制详细解析  
[http://blog.jobbole.com/67286/](http://blog.jobbole.com/67286/ "深入安卓Package Manager和Package Installer")&emsp;&emsp;深入安卓Package Manager和Package Installer  
[https://android.googlesource.com/platform/frameworks/base/+/483f3b06ea84440a082e21b68ec2c2e54046f5a6/services/java/com/android/server/pm/PackageManagerService.java](https://android.googlesource.com/platform/frameworks/base/+/483f3b06ea84440a082e21b68ec2c2e54046f5a6/services/java/com/android/server/pm/PackageManagerService.java "PackageManagerService.java源码")&emsp;&emsp;PackageManagerService.java源码  
[http://mycoding.iteye.com/blog/1075114](http://mycoding.iteye.com/blog/1075114 "APK安装过程及原理详解")&emsp;&emsp;APK安装过程及原理详解  
[http://www.bubuko.com/infodetail-958486.html](http://www.bubuko.com/infodetail-958486.html "Android系统中的 packages.xml与packages.list")&emsp;&emsp;Android系统中的 packages.xml与packages.list  
