# Android安全测试

## 基础介绍

### Android平台、组件等基础知识介绍

#### Android平台架构

Android系统架构采用分层式设计。如果把Android系统看做一层一层的，那么基本可以理解成以下结构（这是其中一种简单的分层方式）：

1. 最上层是应用层（Application层），包含所有应用，比如桌面（桌面也是应用）、电话、设置等，以及我们常用的微信、优酷等应用属于这一层
2. 第二层是应用框架层Framework层），包含了对上层应用的管理和提供开发者所需的应用程序编程接口（API）。
3. 第三层是系统运行库层（包括AndroidRuntime和Libraries）），提供各种各样的库（如C/C++C++）使上一层“看起来”更简单
4. 第四层是硬件抽象层，主要负责将硬件资源抽象成系统资源并管理这些资源，方便兼容不同硬件提供厂商；
5. 最底层是Linux内核层（包括硬件驱动）。

![img](https://upload-images.jianshu.io/upload_images/7534136-c9bf793b4f21bad8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200)

#### Android应用安装包文件APK

APK是安卓应用的后缀，是AndroidPackage的缩写，即Android安装包apk。APK是类似SymbianSis或iOSipa文件格式。通过将APK文件直接传到Android模拟器或Android手机中执行即可安装。apk文件和sis一样，把android开发环境编译的工程打包成一个安装程序文件即为apk

#### AndroidAPK文件结构

APK文件其实是zip格式，但后缀名被修改为apk，通过UnZip解压后，可以看到部分文件。

1. AndroidManifest.xml是编译之后程序全局配置文件。
2. METAINF内存放签名信息。assets和res目录存放资源文件。
3. Dex是DalvikVMexecutes的全称，即AndroidDalvik执行程序，并非传统JavaJVM的字节码而是Dalvik字节码
4. Lib目录存放应用需要的so库文件。
5. Resources.arsc为编译后的资源索引文件。

#### Android开发环境和工程目录

Android开发工具官方推荐为Androidstudio，较早之前也是用eclipse开发，具体项目结构至少由以下几部分构成。

1. AndroidManifest.xml是程序全局配置文件源代码。
2. assets和res目录存放资源文件，在编译的时候原样拷贝。
3. 所有代码都存放于src文件夹。
4. Gradle文件为编译脚本

#### Manifest

AndroidManifest.xml是整个应用的主配置清单文件，其内部有 包名、版本号、组件、权限 等信息内容。清单文件是用来记录应用相关的配置信息的 。Uses permission 用来声明程序使用的权限， 权限都是必须的，特别是那些需要付费或者有安全问题的服务。

```xml
<!-- 这是Google官方示例中的teapots项目中的一个文件AndroidManifest.xml-->
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.sample.teapot"
          android:versionCode="1"
          android:versionName="1.0.0.1" >
  <uses-feature android:glEsVersion="0x00020000"></uses-feature>
    <!--一 个清单文件只能包含一个application 节点， application 可以设置图标android:icon ）、标题 android:label ）、主题样式（ android:theme ）。-->
    <application
      android:allowBackup="false"
      android:fullBackupContent="false"
      android:supportsRtl="true"
      android:icon="@mipmap/ic_launcher"
      android:label="@string/app_name"
      android:theme="@style/AppTheme"
      android:name="com.sample.teapot.TeapotApplication"
      >
    <!-- Our activity is the built-in NativeActivity framework class.
         This will take care of integrating with our NDK code. -->
    <activity android:name="com.sample.teapot.TeapotNativeActivity"
              android:label="@string/app_name"
              android:configChanges="orientation|keyboardHidden">
      <!-- Tell NativeActivity the name of our .so -->
      <meta-data android:name="android.app.lib_name"
                 android:value="TeapotNativeActivity" />
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>
  </application><!--application 标签内可以包含各式各样的组件例如 android 的四大组件 activity 、 service 、 contentprovider 和broadcastreceiver 这样的标签容器，用来去指定应用程序的组件 。-->
</manifest>
```

#### Android四大组件

AndroidAndroid开发四大组件分别是：

- 活动（ Activity) 用于表现功能。
- 服务（ Service) 后台运行服务，不提供界面呈现。
- 广播接收器（ BroadcastReceiver ))：用于接收广播。
- 内容提供商（ Content Provider 支持在多个应用中存储和读取数据，相当于数据库。

#### Activity

Android中 Activity 是所有程序的根本 所有程序的流程都运行在Activity 之中 Activity 可以算是开发者遇到的最频繁 也是 Android当中最基本 的组件之一 。 在 Android 的程序当中 Activity 一般代表手机屏幕的一屏 。 如果把手机比作一个浏览器 那么 Activity 就相当于一个网页 。 在 Activity 当中可以添加一些 Button 、 Check box 等控件 。可以看到 Activity 概念和网页的概念相当类似 。 一般一个 Android 应用是由多个 Activity 组成的 。 这多个 Activity 之间可以进行相互跳转，例如按下一个 Button 按钮后可能会跳转到其他的 Activity 。 当打开一个新的屏幕时之前一个屏幕会被置为暂停状态并且压入历史堆栈中 。

#### Service

Service也是 android 系统中的一种组件 它跟 Activity 的级别差不多 但是它只 能后台运行 并且可以和其他组件进行交互 。 Service 是没有界面的长生命周期的代码 。比如 打开 一个音乐播放器的程序 这个时候若想上网了 那么 打开 Android 浏览器 这个时候虽然已经进入了浏览器这个程序 但是 歌曲播放并没有停止 而是在后台继续一首接着一首的播放 。 其实这个播放就是由播放音乐的 Service 进行控制 。 当然这个播放音乐的 Service 也可以停止 例如 当播放列表里边的歌曲都结束 或者用户按下了停止音乐播放的快捷键等 。 Service 可以在和多场合的应用中使用 比如播放多媒体的时候用户启动了其他 Activity 这个时候程序要在后台继续播放 比如检测SD 卡上文件的变化 再或者在后台记录地理信息位置的改变 等等 。

#### BroadcastReceiver

在Android 中， Broadcast 是一种广泛运用的在应用程序之间传输信息的 机制（用来做异步处理）。 而 BroadcastReceiver 是对发送出来的 Broadcast 进行过滤接受并响应的一类组件。可以使用 BroadcastReceiver 来让应用对一个外部的事件做出响应 。例如 ，当电话呼入这个外部事件到来的时候，可以利用 BroadcastReceiver 进行处理 。当 下载一个程序成功完成的时候，仍然可以利用 BroadcastReceiver 进行处理。 BroadcastReceiver 不能生成 UI ，也就是说对于用户来说不是透明的，用户是看不到的 。但是 BroadcastReceiver 通过 NotificationManager 来通知用户这些事情发生了 。

#### Content Provider

在Android 中 ，对数据的保护是很严密的，除了放在 SD 卡中的数据， 一个 应用本身所 持有的数据库、文件等 内容（路径为 /data/ 应用包名 都是不允许其他直接访问的。 Android 不会 真的把每个应用都做成一座孤岛，它为所有应用都准备了一扇窗，这就是 Content Provider 。应用想对外提供的数据，可以通过派生 Content Provider 类， 封装成一枚 Content Provider ，每个 Content Provider 都用一个 uri 作为独立的标识，形如： content:// com.xxxxx 。所有东西看着像 REST 的样子，但实际上，它比 REST 更为灵活 。

## 工具简介

### 介绍使用的工具（静态分析和动态调试）

#### 工具列表分类

- Jeb-静态逆向分析工具
- apktool-apk-反编译和打包工具
- tcpdump-tcp-数据抓包工具
- Xposed-android-hook-框架
- Frida-跨平台-hook-框架
- JustTrustMe-xposed-框架下信任所有证书的插件实现
- Burpsuite-基于代理实现的抓包和分析-工具
- IDA-二进制逆向工具和调试工具

## Hook

### 如何使用xposed和frida来辅助测试

#### Xposed

Xposed框架是一个由 xda 开发的 hook 框架，并且有许多基于这个框架的优秀插件（功能模块）。这些插件配合 Xposed框架可以在不修改 APK 的情况下影响程序的运行，几乎可以达到为所欲为的目的，对APK 实现界面修改、逻辑修改、数据修改。比如：直接把 APP 的界面改成自己想要的样子，去掉界面里不喜欢的东西。自动抢红包，消息防撤回，步数修改等等。不过这一切的先决条件就是 Xposed 需要拥有最高权限 。它的原理是替换 Android 的孵化器进程 zygote ，然后把所有方法注册成 native 进行hook 。

#### Frida

frida是一款基于 python + javascript 的 hook 框架，可运行在android 、 ios 、 linux 、 winosx 等各平台，主要使用动态二进制插桩技术。插桩技术是指将额外的代码注入程序中以收集运行时的信息，可分为两种 源代码插桩、二进制插桩 。二进制插桩指额外代码注入到二进制可执行文件中，又可以将其细化为 静态二进制插桩、动态二进制插桩 。静态二进制插桩是在程序执行前插入额外的代码和数据，生成一个永久改变的可执行文件。动态二进制插桩则是在程序运行时实时地插入额外代码和数据，对可执行文件没有任何永久改变。 Frida 在 Android 平台上需要配置一个 frida server ，二进制动态插桩技术的原理是使用了ptrace 附加到目标进程上，访问进程的内存、 Hook 和跟踪以及拦截函数等等。

#### 为什么抓不到包？

- 配置错误
- Https 无法抓包
- Https 证书绑定情况下无法抓包
- 客户端代码中添加了不允许抓包的代码
- 数据加密和双向认证

#### Https证书 绑定

Https证书 绑定指客户端在代码内部校验了服务器证书，当发现证书不匹配的时候不发起请求，用来对抗中间人攻击。这种情况可以使用 xposed 插件来抹去证书校验代码，不管校验是否成功，都按成功处理。

#### 数据加密

部分客户端与服务器交互的数据经过加密处理，导致渗透测试人员无法开展测试工作，这种情况有两种结局方案。第一，可以逆向加密算法，然后在 burp 中开发一个解密插件。第二可以尝试使用 hook 框架 hook 加密函数，拿到明文，然后与 burp 进行交互。

## 脱壳

### 如何进行脱壳

#### DexExtractor脱壳 原理 DVM

1. 一 个 DEX 文件需要被解析成 Dalivk 虚拟机能操作对象 DvmDex 解析过过程中就必须调用 DexFile *dexFileParse const u1* data, size_t length, int flags) 。
2. dexFileParse 函数原型如下： DexFile * dexFileParse const u1* data, size_t length, int flags)data ：文件就是 DEX 文件经过优化后再内存中数据的首地址。 length ：文件就是 DEX 文件经过优化后再内存中数据的长度
3. DexExtracctor 就是重写 dexFileParse 函数，在原函数实现基础上，将内存中 DEX 文件中数据经过Base64 加密，写到移动设备的 SD 卡。
4. 使用 Base64 加密是为了对有些加固代码，对 read.write 函数进行 hook 。

