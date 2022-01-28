# Hook的概念

Hook 技术又叫做钩子函数，在系统没有调用该函数之前，钩子函数先得到控制权，这时钩子函数既可以加工处理（改变）该函数的执行行为，还可以强制结束消息的传递。

## HookDemo

```java
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        showMsg("showmsg...");
    }
    public void showMsg(String msg) {
        Toast.makeText(this,msg,Toast.LENGTH_LONG).show();
    }
    
    Java.perform(function() {//这是frida的main，所有的脚本必须放在这个里面。
        // 这里写Hook代码的具体逻辑
        var mainClazz = Java.use("com.funnysec.demo1.MainActivity");
        mainClazz.showMsg.implementation = function(v1) {
            return this.showMsg("frida4androidhook");
        }
    });
} 
```

# Frida基本用法

## Frida

Frida是个轻量级别的hook框架，Frida的核心是用C编写的，并将Js引擎注入到目标进程中，在这些进程中，JS可以完全访问内存，挂钩函数甚至调用进程内的本机函数来执行。支持Windows、Linux、 IOS等平台

## Frida框架环境搭建

准备一款模拟器(夜神模拟器)，也可以是真机(需要root)。

```cmd
C:\>pip3 install frida
Requirement already satisfied: frida in e:\python\python3.9\lib\site-packages (15.1.10)
Requirement already satisfied: setuptools in e:\python\python3.9\lib\site-packages (from frida) (57.4.0)

C:\>pip3 install frida-tools
Requirement already satisfied: frida-tools in e:\python\python3.9\lib\site-packages (10.4.1)
Requirement already satisfied: pygments<3.0.0,>=2.0.2 in e:\python\python3.9\lib\site-packages (from frida-tools) (2.10.0)
Requirement already satisfied: prompt-toolkit<4.0.0,>=2.0.0 in e:\python\python3.9\lib\site-packages (from frida-tools) (3.0.22)
Requirement already satisfied: colorama<1.0.0,>=0.2.7 in e:\python\python3.9\lib\site-packages (from frida-tools) (0.4.4)
Requirement already satisfied: frida<16.0.0,>=15.0.0 in e:\python\python3.9\lib\site-packages (from frida-tools) (15.1.10)
Requirement already satisfied: setuptools in e:\python\python3.9\lib\site-packages (from frida<16.0.0,>=15.0.0->frida-tools) (57.4.0)
Requirement already satisfied: wcwidth in e:\python\python3.9\lib\site-packages (from prompt-toolkit<4.0.0,>=2.0.0->frida-tools) (0.2.5)
```

命令行中输入`frida --version`命令，检查是否有显示版本号，[下载frida-server](https://github.com/frida/frida/releases)，模拟器下载x86架构，真机下arm架构，**frida-server版本需要和frida一致**，将模拟器中的/bin/目录添加到系统环境变量中

```cmd
C:\>adb devices
List of devices attached
127.0.0.1:62001 device

C:\>adb push E:\frida-server-15.1.14-android-x86 /data/local/tmp/           
[100%] /data/local/tmp/frida-serverx86

C:\>adb shell
root@shamu:/ # cd data/local/tmp/
root@shamu:/ # chmod 777 frida-server-15.1.14-android-x86
root@shamu:/data/local/tmp # ls -l
-rwxrwxrwx root     root     46416812 2021-12-29 15:33 frida-server-15.1.14-android-x86
```

将frida-server上传至模拟器内，因为Android系统的限制，Android系统中并不支持+rwx这种命令格式，所以用chmod 777命令，开启另外一个CMD窗口执行端口转发使其与主机互通`adb forward tcp:27042 tcp:27042`，`adb forward tcp:27043 tcp:27043`

## 服务端(frida-server)

不需要用户关心，运行起来即可。

## 客户端(frida-tools)

提交要注入的代码到服务端，接受服务端发来的消息，一些工具等。

### Frida CLI

`frida -U -f com.xxx.xxxx -l hook.js –no-pause`

- -U 指定对USB设备操作
- -l 指定加载一个hook脚本
- -f 表示由frida来帮助你启动apk程序
- --no-pause 省略输入 %resume

### Frida-ps

`frida-ps -Uai`

- --help 查看帮助
- -U 指定USB设备
- -a 显示存活的应用
- -i 显示全部的应用

```cmd
C:\>frida-ps -Uai
 PID  Name           Identifier
----  -------------  ----------------------------------
3675  FDex2          formatfa.xposed.Fdex2
3292  HttpCanary     com.guoshi.httpcanary
3448  Inspeckage     mobi.acpm.inspeckage
3403  drozer Agent   com.mwr.dz
   -  下载             com.android.providers.downloads.ui
   -  图库             com.android.gallery3d
   -  文件管理器          com.cyanogenmod.filemanager
   -  设置             com.android.settings
```

## Java构造函数hook

```java
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ShowMsg showMsg = new showMsg("m7");
        Toast.makeText(this,showMsg.getMsg(),Toast.LENGTH_LONG).show();
    }
class ShowMsg{
    private String msg;
    private Context ctx;
    ShowMsg(String msg){
        this.msg=msg;
    }
    public String getMsg(){
        return "hacker by"+this.msg;
    }
}
```

hook脚本

```java
  Java.perform(function() {
	var showMsgClazz = Java.use("com.funnysec.demo1.ShowMsg");
	showMsgClazz.$init.implementation = function(v1) {
		return this.$init("ljr 十年经验，老安全专家！");
	}
});
```

## java重载函数hook

```java
public class MainActivity extends AppCompatActivity{
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        showMsg("showmsg2...");
    }
    public void showMsg() {
        Toast.makeText(this,"showmsg",Toast.LENGTH_LONG).show();
    }
    public void showMsg(String msg) {
        Toast.makeText(this,msg,Toast.LENGTH_LONG).show();
    }
} 
```

hook脚本1

```java
Java.perform(function() {
	var mainClazz = Java.use("com.funnysec.demo1.MainActivity");
	mainClazz.showMsg.implementation = function() {
		return "Hook showMsg2";
	}
});
```

hook脚本2

```java
Java.perform(function() {
	var mainClazz = Java.use("com.funnysec.demo1.MainActivity");
	mainClazz.showMsg.overload('java.lang.String').implementation = 
	function() {
		return this.showMsg("Hook showMsg2");
	}
});
```

## python的支持

```python
import frida, sys
def message(message, data):
    print(message)
hookCode = """
    if(Java.available){
        Java.perform(function(){
            var kClazz = Java.use("com.bytedance.common.utility.k");
            var encVal = kClazz.c("18888888888");
            send(encVal);
        });
    }
"""
process = frida.get_remote_device().attach('com.dragon.read')# 指定一个进程名
# process = frida.get_device_manager().add_remote_device('127.0.0.1:8888').attach('com.funnysec.fridatest')
script = process.create_script(hookCode)# 往进程中注入代码
script.on("message", message)# 回调函数,配合frida中的send函数
script.load()# 加载注入的代码
sys.stdin.read()# 让脚本处于阻塞状态，不让它执行结束
```

# 脱壳

一代整体型壳: 采用Dex整体加密，动态加载运行的机制。

二代函数抽取型壳: 粒度更细，将方法单独抽取出来，加密保存，解密执行。

三代VMP、Dex2C壳: 独立虚拟机解释执行、语义等价语法迁移，强度最高。

[frida-dexdump](https://github.com/hluwa/FRIDA-DEXDump)基于frida实现，在内存中暴力搜索 dex035也就是dex文件头，从而抽取处dex文件。

1. 安装: python3 -m pip install frida-dexdump
2. 启动frida-server端
3. 启动要脱壳的应用
4. 执行命令进行脱壳 frida-dexdump
5. 使用jar cvf命令合并dex文件

# hook加解密

在App抓包过程中可能会遇到一些参数是加密的情况，可以通过Hook技术对加密函数进行调用，如果客户端存在解密函数也可以尝试对密文进行解密，配合fuzz对目标进行测试。

![图片1](F:\file\知识\0-安全服务交付\图片1.png)

使用Hook对加密函数进行调用，计算出被加密过后的值。使用jadx-gui对apk进行反编译，尝试搜索关键字或者页面请求的接口地址。

![图片2](F:\file\知识\0-安全服务交付\图片2.png)

gVar.a的值为mobile，也就是未加密过的手机号。在外出还调用了k.c，可能就为加密函数。对k.c进行hook

![图片3](F:\file\知识\0-安全服务交付\图片3.png)

可以封装成一个函数，实现主动调用。

```java
function enc(v) {
	Java.perform(function()) {
        var kClazz = Java.use("com.bytedance.common.utility.k");//找到包的位置
        var encV = kClazz.c(v);//获取原来的值
        console.log(encV);//加密后的值
	});
}
```

# 代理检测绕过

有些App会检测是否开启了网络代理，如果开启APP将无法正常使用。此时需要通过找到关键的判断函数并使用Hook技术进行绕过。有壳的APP先脱壳，方便分析逻辑。使用jadx-gui通过搜索关键字找到关键位置

![图片4](F:\file\知识\0-安全服务交付\图片4.png)

查看查找用例，类似于交叉应用。MyUtil.processWifiProxy()的调用在一处匿名函数内。匿名函数是否实例化的关键在于外层的if判断，当MyUtil.isWifiProxy()为true时将弹出提示信息。

![图片5](F:\file\知识\0-安全服务交付\图片5.png)

![图片6](F:\file\知识\0-安全服务交付\图片6.png)

Hook思路: 让isWifiProxy()函数永远返回false，这样if将不会成立，从而不会出现弹窗。

```java
Java.perform(function()) {
	var myUtil = Java.use("com.htjy.app.common_work.utils.MyUtil");
	myUtil.isWifiProxy.implementation = function() {
		return false;
	}
});
```

# ssl pinning证书绕过

提示网络不通或者异常之类的，直接使用[ssl pinning绕过脚本](https://github.com/horangi-cyops/flutter-ssl-pinning-bypass)，大部分情况都能绕过，除非自己实现了一些底层的校验机制。

# Bride插件

[Brida](https://github.com/federicodotta/Brida)是BurpSuite的插件，它充当Burpsuite和Frida之间的桥梁。Brida0.4集成了由许多Frida的Hook脚本，0.3版和0.4版使用起来有些差距。

## 安装

需要python，Burpsuite，[nodejs](https://nodejs.org/zh-cn/download/)，[Brida.jar](https://github.com/federicodotta/Brida)

```cmd
C:\>pip3 install Pyro4
WARNING: Ignoring invalid distribution -ip (e:\python\python3.9\lib\site-packages)
WARNING: Ignoring invalid distribution -ip (e:\python\python3.9\lib\site-packages)
Collecting Pyro4
  Downloading Pyro4-4.82-py2.py3-none-any.whl (89 kB)
     |████████████████████████████████| 89 kB 634 kB/s
Collecting serpent>=1.27
  Downloading serpent-1.40-py3-none-any.whl (9.6 kB)
WARNING: Ignoring invalid distribution -ip (e:\python\python3.9\lib\site-packages)
Installing collected packages: serpent, Pyro4
WARNING: Ignoring invalid distribution -ip (e:\python\python3.9\lib\site-packages)
WARNING: Ignoring invalid distribution -ip (e:\python\python3.9\lib\site-packages)
Successfully installed Pyro4-4.82 serpent-1.40
```

nmp安装

```cmd
C:\Users\>npm config set prefix E:\nodejs\node_modules\npm\  
C:\Users\>npm config get prefix
E:\nodejs\node_modules\npm
C:\Users\>npm install frida-compile

up to date, audited 256 packages in 4s

44 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

## 功能面板

### Configurationa

配置一些环境信息。启动frida框架，当以Spawn application方式hook时用包名，以Attach application时用PID。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191022223545691.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTE5MDg5Nw==,size_16,color_FFFFFF,t_70)

### JS Editor

Hook脚本编写。JavaScript的编辑器相当于一个记事本。

### Hooks and functions

调用内置的Hook脚本。

### Graphical analysis

可查看Apk中用到的类和库资源。双击一个类将获得类对应的函数

Inspect: 在函数上添加一个检查钩，当目标程序调用该函数时打印该函数的参数和返回值

Inspect with backtrace! 添加检查钩，并打印调用栈。

Change return value 修改函数返回值。

![图片8](F:\file\知识\0-安全服务交付\图片7.png)

### Graphical hooks

钩子可以在该模块中进行查看和删除、禁用、启用等。

### Custom plugins

创建一些按钮的UI方便调用以及和Burpsuite的其他模块中进行联动操作

![图片8](F:\file\知识\0-安全服务交付\图片8.png)

使用icontextmenu类型可以在发包器中右键调出插件

### Debug export

调用Frida导出的函数

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191023001946589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTE5MDg5Nw==,size_16,color_FFFFFF,t_70)

实战相关可以参考扩展文章：https://blog.csdn.net/weixin_39190897/article/details/102691898
