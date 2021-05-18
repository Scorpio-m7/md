# iOS应用程序安全风险及漏洞解析

## 基础介绍

### iOS 系统介绍

- 移动操作系统
- 基于在 XNU 内核， Darwin OS
- iOS 是闭源操作系统
- 使用 OC 与 Swift 作为开发语言
- 构建在 Cocoa Touch API

### iOS 应用沙盒

- 基于 TrustedBSD MAC 强制访问框架
- 第三方应用程序运行在 mobile 用户权限下
- 只有系统应用以 root 用户运行
- 应用程序只能访问自己的文件和数据
- IPC 通信被限制

### Jailbreak

不完美越狱：设备重启后，之前的越狱状态会失效，需要手动重新越狱
完美越狱：越狱后设备重启，设备仍处于越狱状态，即重启不会破坏越狱环境

- 可越狱系统： 7.0 ~ 12.4 ，部分不可越狱
- 越狱软件，例如 unc0ver iOS11.0 ~ 12.3.x ~ 12.4.1
- checkra1n ，搭载 A5 ~ A11 处理器的设备 ~ iPhone X

越狱后可以安装Cydia.app

> 越狱目的：获取root 权限，如果没有越狱，所有的操作就只能局限于沙盒 ( 而越狱之后可以访问设备的整个文件系统、更改系统外观和功能、编写命令行工具、 root App 及动态注人动态库到指定应用等。虽然越狱后可以获取 root 权限，但是原来在沙盒内的应用在越狱之后还是在沙盒内

### 工具介绍

#### macos

- iTerm2: 代替默认的 Terminal ，提供了很多高级设置，例如自动补全、高亮等
- [oh my zsh](https://github.com/jeremyFreeAgent/oh-my-zsh-powerline-theme): 可以自定义主题、 Git 显 7K 、 Tab 补全等
- [Alfred](https://www.alfredapp.com): 快速打开软件，自定义脚本执行 
- [libimobiledevice](https://github.com/libimobiledevice/libimobiledevice): 提供了很多能与 iOS 交互的工具，例如端口映射査看日志 安装应用，查看应用 bundleid等 。
- [class dump](https://github.com/nygard/class-dump), 经过编译的 Objective-C 二进制文件中含有所有的类、协议、方法和实例变量等信息，工具可以获取这些信息 ，在分析过程中，通过该工具生成的头文件就可以快速找到想要的方法或属性。
- 转发端口 iproxy 22 2222
- theos, 越狱开发工具包
- IDA, Hopper 经典反汇编工具

#### ios

- OpenSSH 就可以通过 Cydia 装在手机上，远程连接。
- Apple File Conduit "2" , 访问越狱设备的文件系统
- Cydia Substrate, 一个用于持续性动态篡改的框架，允许第三方开发者在越狱系统的方法里打一些运行时补丁和扩展一些方法，是开发越狱插件的基石。
- appsync, 直接修改一个应用的结构或文件会破解应用本身的签名信息，安装修改后的应用会“FailedtoverifycodesignatureofXXX” 这样的错误。安装 appsync ，让系统不再验证应用的签名。
- [Cycript](http://www.cycript.org), 使用 OC 和 js 组合语法查看 修改运行时 App 内存信息工具，其官网提供了它的一些经典使用方法 ,Cydia 中搜索 Cycript 并安装
- flex3, 第三方修改器 动态修改工具。
- passionfruit iOS 应用安全分析
- [Frida](https://frida.re/)，跨平台动态分析套件，使用 javascript 脚本注入进程，执行二进制插装。可与调试器共存。在脚本和原生函数中实现了双向 bridge 。可 hook 函数调用修改参数，也可使用脚本调用原生函数实现复杂功能。内置对 Objective C 运行时支持
- 其他 ,dv cmds,iFIle ...

### 常见检测点

- 获取 IPA 文件，检测二进制
- 检测本地数据存储 (caches, binary cookies, plists, database)
- Bypass certificate pinning(SSLKill Switch)
- 分析 HTTP(s) 服务端漏洞
- 越狱检测
- 分析 App 逻辑
- 其他：日志输出、应用程序缓存快照、键盘

## iOS 应用安全问题

### 二进制文件

解密二进制文件：dumpdecrypted -- decrypt the application from AppStore

#### 应用程序特征

###### IPA 文件

- IPA 文件是 ZIP 文件
- 包含可执行二进制文件，图片以及其他初始化数据

###### 静态分析

- 通常是 FAT 文件 包含 ARMv7(s) 和 ARM64 架构
- 分析需要解密（砸壳）位二进制文件

#### 应用程序路径定位

- 所有的系统应用在 /Applications
- 使用 installipa 获取应用程序信息

#### 砸壳

#### ARC

```bash
$ otool -Iv CaifuDriver | grep release
```

#### SSP

```bash
$ tool -Iv CaifuDriver | grep stack
```

#### PIE

```bash
$ otool -hv CaifuDriver
```

### 敏感信息

#### 文件存储

- 安全问题
  - 存储的数据包括自动登录信息、 Cookie 、手势密码
  - NSUserDefaults 、数据库文件、普通文件
  - iOS 8.3 以下系统版本或越狱设备上， App 数据文件可导出
- 解决方案
  - 重要数据存储至 Keychain
  - 数据库文件可设置 sqlite key 进行数据库加密
  - 避免明文存储

#### 日志输出

- 安全问题
  - NSLog 或第三方库 Log 中打印调试信息
  - 输出敏感信息或函数调用流程
- 解决方案
  - 使用宏来控制开发版本和发布版本的日志输出
  - 注意关闭第三方 SDK 的日志，一般都有相关的接口

#### 手势密码

- 安全问题
  - 手势密码被反解得到明文
  - 手势密码相关数据可被修改
- 解决方案
  - 本地认证应采用 设备相关信息 密码 进行 hash
  - 多次验证失败的情况，设置尝试次数阈值
  - 手势密码相关信息加密存储

#### 后台快照

- 安全问题
  - App 切换至后台时会保存当前界面的快照
  - 泄露界面包含的敏感信息
- 解决方案
  - [UIApplicationDelegate applicationDidEnterBackground] 处理

### 数据加密

#### 加密算法

- 安全问题
  - 算法自身问题
- 解决方案
  - 避免使用自定义加密算法
  - 使用 AES 、 DES 等算法
  - 针对数据安全性要求高的场景，使用非对称加密

#### 算法关键数据

- 安全问题
  - 算法 KEY 生成或存储不当
  - 对称加密 KEY 硬编码
  - 攻击者可逆向分析程序获取硬编码数据
- 解决方案
  - 以设备相关的信息作为基础生成数据 (identifierForVendor)
  - 算法关键数据需具有独立性

### 网络交互

#### HTTPS 认证

逐渐放宽服务器的认证要求，并检测流量是否成功通过每个阶段的Burp 代理
1. Set Burp in proxy settings, make sure that SSL Killswitch is disabled and that Burp Profile is *not* installed → no certificate validation
2. Install Burp Profile (certificate) →no certificate pinning
3. Enable SSL Killswitch →certificate pinned
4. Bypass certificate pinning manually

#### 数据传输

- 安全问题
  - 敏感数据使用 HTTP 传输
  - 使用 HTTPS ，未进行服务器证书校验
- 解决方案
  - 使用 HTTPS 应对数据安全性较高的场景，在客户端进行证书校验
  - 对于 HTTP ，避免明文传数据，在必要的情况下使用非对称加密

#### WebView & Javascript 交互

- 安全问题
  - 注册获取信息（ GPS 等）接口，恶意网页通过 js 调用获取信息
  - 注册 Native 接口缺陷，恶意 js 代码导致 App 崩溃
- 解决方案
  - 使用 safari 打开第三方链接
  - 避免注册涉及敏感信息的接口，如获取地理信息位置等
  - 使用 WebView 加载页面并涉及 js 调用时，进行白名单检查

### 应用完整性

#### 越狱环境检测

- 安全问题
  - 提示用户处于越狱环境中
- 解决方案
  - 基于文件系统的检测
  - 基于 API 调用的检测

##### 基于文件的检测

- /Library/MobileSubstrate/MobileSubstrate.dylib
- /System/Library/LaunchDaemons/com.saurik.Cydia.Startup.plist
- /var/cache/apt
- /var/lib/cydia
- /bin/sh
- /etc/sshd_config
- /Applications/Cydia.app

##### 基于 API 调用的检测

- fork ：在沙盒中 fork 是被限制调用的，检测返回值
- dyld 系列函数：检测进程都加载了哪些 dylib
  - _dyld_image_count
  - _dyld_get_image_name

#### 进程反调试

- 威胁
  - 攻击者通过调试，分析程序逻辑
- 解决方案
  - sysctl 检测进程状态
  - ptrace 禁止调试器附加

#### 进程反注入

- 威胁 (dylib 注入)
  - 通过 Hook 关键函数，可窃取应用数据
  - 辅助攻击者逆向分析应用程序流程
- 解决方案
  - 限制模式
  - 设置链接选项 -Wl, -sectcreate,__RESTRICT,__restrict,/dev/null

### 代码实现缺陷

#### 逻辑缺陷

bp抓包修改包中内容

#### 数据解析

- JSON 类型混淆
- 解决方案
  - 保证数据源可信（ HTTPS）
  - 验证 JSON 数据合法性（ JSON schema）
  - 在使用时验证数据类型的合法性（扩展 NSDictionary）

### 第三方库

- 第三方库可能存在的安全隐患
  - 拒绝服务
  - 信息泄露
  - 逻辑缺陷
- 解决方案
  - 及时更近安全更新

#### FFmpeg

- CVE 2016 1897/CVE 2016 1898 远程文件读取
- 可能导致沙盒内文件被读取并上传给攻击者

#### AFNetworking

- 低于 2.5.3 存在证书校验绕过的问题
- 由于代码实现的逻辑问题引起
- 攻击者使用非法证书可实现 HTTPS 中间人攻击

### 其他

#### 开发环境安全性

- XCodeGhost 事件
- 开发环境工具链纯净
- 解决方案：使用官方渠道下载的开发环境

#### 工程编译打包

- 资源打包不当，信息暴露
  - 泄露内部开发、设计文档
  - 泄露团队成员信息及联系方式
  - 泄露源码
  - 泄露开发密钥及 mobileprovision 文件
- 解决方案
  - 管理好 Xcode 的 “Build Phases” 、 “Copy Bundle Resources"
