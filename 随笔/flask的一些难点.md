### flask的一些难点

**创建蓝图对象**

> blue = Blueprint('first',__name__)

**注册蓝图**

> app.register_blueprint(blueprint=蓝图名)

**导入配置类**

> app.config.from_object(类名)

SQLAlchemy配置与初始化

**配置**

> SQLALCHEMY_TRACK_MODIFICATIONS = True

> SQLALCHEMY_DATABASE_URI = 'sqlite:///test.db

**初始化**

> db = SQLAlchemy()

    def init_ext(app):
    	db.init_app(app)
路由中指定请求服务器方式  session设置  重定向与反向解析

    @blue.route('/checkout/',methods=['GET','POST'])
    def checkout():
    	username = request.form['username']
    	session['username'] = username
    	return redirect(url_for('first.welcome'))  #注意需蓝图名.函数名
模版中表单的反向解析

```
<form method='post' action={{ url_for('first.check_user') }}>
```

模版中的继承

```
{% extends 'base.html' %}
```

继承父模块里的代码但不覆盖

```
{{ super() }}
```

插入另一个模版中的代码块

```
 {% include 'content.html' %}
```

在模版中定义函数

```
{% macro test() %}
  <p>呵呵呵呵</p>
{% endmacro %}
```

调用其他模版中函数

```
{% from 'content.html' import test %}
{{ test() }}
```