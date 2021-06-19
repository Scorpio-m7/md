# Linux命令风格；文件系统

## 系统调用

- 系统调用以c语言函数调用的方式提供

- 操作系统内核提供的编程界面

  应用程序（ap）和操作系统（kernel）进行交互的唯一手段。

  例如：文件操作的open，read，write，close

- 系统调用与库函数在执行上有区别

  库函数在用户态下运行，和可执行文件是一体的，在一个文件中执行。系统调用是调用操作系统里的功能，工作在系统特权状态，使用cpu的INT软中断指令，在操作系统内核完成。

  例如：获取进程ID的getpid()与字符串拷贝函数strcpy()

  CPU的INT指令（软中断）与CALL指令（子程序调用）

- 库函数对系统调用的封装（API）

  目的：执行效率更高，调用界面更方便

  例如：库函数printf对系统调用write的封装

  库函数malloc/free对系统调用sbrk的封装

- 整形全局变量errno

  标准库为errno保留存储空间，系统调用失败后填写错误代码，记录失败原因

  #include<errno.h>之后，就可以直接使用变量errno

  errno.h头文件定义了许多有E前缀的宏，例如EACCES，EIO，ENOMEM，EINTR。相关系统调用的手册页中有出错说明

  在man命令给出的手册页中有ERRORS一节介绍出错原因，如man recv

- strerror

  char *strerror(int errno);

  errno是个整数，便于程序识别错误原因，不便于操作员理解。库函数strerror将数字形式的错误代码转换成一个可阅读的字符串

- printf的%m

  是printf类函数格式化字符串中的%m会被替换成上次系统调用失败的错误代码对应的消息（message）

## 访问i节点和目录

- 系统调用stat

  得到指定路径名的文件的i节点

  stat的结构体

  + st_dev：存储该文件的块设备的设备号，包括主设备号与次设备号

    例如stat命令显示文件Device:821h/2081d，/前面的是十六进制，代表主设备号8（高字节），此设备号33（低字节），/dev/sdc1

  + st_mode域：16比特，文件的基本存储权限和SUID/SGID权限（11比特）及文件的类型（5比特）

    + 文件类型：

      S_IFREG:普通磁盘文件

      S_IFDIR:目录文件

      S_IFCHR:字符设备文件

      S_IFIFO:管道文件

      S_IFLNK:符号连接文件

  + st_size与st_blocks

    程序可以通过st_size获取文件大小

    一般情况：st_size<st_blocks*512

    稀疏文件：st_size>st_blocks*512

  + st_ctim，st_atim，st_mtim

    Linux中存储这三个时间的精度为纳秒

    “c改变”：i节点信息变化。写文件，修改权限/link数/换文件主会使得c变化（m变，c也变）

    “a访问”：读，执行（不早于m时间）

    “m修改”：文件内容修改。写文件

- 系统调用fstat

  得到已打开文件的i节点（如果文件已经打开，i节点就已经复制到操作系统内核）

- 目录访问

  + 使用库函数opendir，打开目录得到句柄

  + readdir获取一个目录项

    dirent结构体：记录i节点号和文件名（d_ino和d_name成员），返回值指针指向dirent结构体（返回null表示已经读到目录尾部）

  + 访问结束：用closedir关闭句柄

# 文件和目录的权限；Shell的基本机制

## 文件和目录的权限

### 三级权限存在的问题

系统中任一个用户，要么对文件的全部内容具有访问权，要么不可访问文件。有的情况下，很不方便，例如用户修改口令文件/etc/passwd和/etc/shadow

### 策略和机制相分离（SUID权限）

文件list.txt的文件主liu给文件query增加SUID权限

```bash
root@kali:~/#chmod u+s query
root@kali:~/#ls -l query
-rws--x--x	1	liu	leader	1234	Dec	10	23:22	query
```

一般情况下，进程的实际UID和有效UID相等

打开文件open()时，系统根据进程的有效UID，与文件的所有者UID之间的关系和文件的权限进行访问合法性验证。可执行程序具有SUID权限，进程的实际UID和有效UID不再相等。实际UID是当权用户，而有效UID为可执行文件的文件主。SUID使得用户可以通过文件主提供的程序，以文件主的权限访问文件，但是这种访问依赖于文件主提供的程序，进行有限的访问

文件主创建query.c，并且添加SUID权限，使用`make query`编译后，其他用户可以`./query`查看list.txt中关于自己的一行

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>

int main(void)
{
    FILE *f;
    char line[512],name[64],*username;
    if (f=fopen("list.txt","r")==NULL)
    {/*打开文件，失败给出信息*/
        printf(*** ERROR: Open file \"list.txt\":%m\n");
        exit(1);
    }
    username=getlogin();/*获取当前登录的用户名*/
    while (fgets(line,sizeof(line),f))/*按行读取*/
    {
        if (line[0]=='#')/*#开头则输出*/
            printf("%s",line);
        else if (sscanf(line,"%s",name)>0 && strcmp(name,username)==0)/*从字符串中取第一个单词，如果和当前用户名相同则输出行*/
            printf("%s",line);  
    }
}
```

