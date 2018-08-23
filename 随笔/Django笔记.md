Django笔记

1、进入虚拟环境

2、创建存放项目目录并进入该目录

3、创建项目

```django-admin startproject 项目名```

进入项目目录，创建应用

```python manage.py startapp 应用名字```

4、启动pycharm

配置数据库



    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'day03',
            'HOST':'localhost',
            'USER':'root',
            'PASSWORD':'dy123321',
            'PORT':'3306'}
        }
在包里导入mysql

import pymysql
pymysql.install_as_MySQLdb()

5、配置模版目录

**[os.path.join(BASE_DIR,'templates')],**

6、配置urls

**url(r'^app/',include('app.urls',namespace='app'))**

**url(r'^$',views.home) 通过ip的地址直接访问主页**

7、配置静态文件

STATIC_URL = '/static/'

STATICFILES_DIRS = [os.path.join(BASE_DIR,'static')]