## manager.py

- 调用createApp()函数，获取app对象，创建manage对象

```manager.py
#调用__init__中的createApp()函数，执行了createApp()函数下的代码
from flask import Flask
from flask_script import Manager
from App import createApp

app = createApp()  #创建app对象
manager = Manager(app)  #创建manager对象
if name == 'main':
	manager.run()	              
```

## __init__.py

- 定义app对象    注册蓝图     导入插件配置的类     调用初始化插件函数

```__init__.py
# 调用初始化插件函数实现app传参
#createApp()函数体中执行了调用ext.py中的init_ext(app)函数
from flask import Flask
from flask_session import Session
from App.ext import init_ext
from App.settings import Config
from App.views.db_blue import db_blue
from App.views.session_blue import blue

def createApp():
# 创建app对象
app = Flask(__name__)

# 注册蓝图
app.register_blueprint(blueprint=blue)
app.register_blueprint(blueprint=db_blue)

# 导入插件配置类
app.config.from_object(Config)

# 调用初始化插件函数并将app作为实参传入
init_ext(app)

return app
```
## settings.py

- 定义一个类，通过定义类属性来设置插件配置

  ```
  class Config:
  	#session插件配置
     	SECRET_KEY = 'sdfsg'
     	SESSION_TYPE = 'redis'    # 指定session以哪种方式持久化
  	#设置session的过期时间，单位是秒。默认是31天
  	#PERMANENT_SESSION_LIFETIME = 120

  	#sqlalchemy插件配置
    	SQLALCHEMY_DATABASE_URI = 'sqlite:///test.db'
    	SQLALCHEMY_DATABASE_URI = 'mysql+pymysql://root:dy123321@localhost:3306/day03'
  ```
## ext.py

- 定义初始化插件函数

  定义一个初始化插件的函数,函数定义一个app形参,在init里面调用函数时把app对象传入

  ```
  from flask_session import Session
  from flask_sqlalchemy import SQLAlchemy
  #创建db对象，在model中使用db对象创建表
  db = SQLAlchemy() 

  def init_ext(app):
    	Session(app)        #session插件初始化
    	db.init_app(app)    #SQLAlchemy插件初始化
  #将app作为实参传入SQLAlchemy函数中,实现插件初始化
  #db = SQLAlchemy()  db.init_app(app)  相当与SQLAlchemy(app) 就是多了一个db对象
  ```
### model.py

- 使用db对象来创表

  ```
  from App.ext import db  #导入db

    class Person(db.Model):

    	id = db.Column(db.Integer,primary_key=True,autoincrement=True)

    	name = db.Column(db.String(128),unique=True,nullable=True)

    	age = db.Column(db.Integer,nullable=False,default=10)
  ```
### views.py

- 使用db.create_all()方法执行创表

  ```
  from flask import Blueprint
  from App.ext import db
  from App.models import Person  #注意：创建表模型必须导入模型中相对的类
  #创建蓝图对象
  db_blue = Blueprint('db',name)

  @db_blue.route('/createtable/')
  def create_table():
    	db.create_all()           # 调用这个方法创建表格
    	return '表格创建成功'
  ```





  ​