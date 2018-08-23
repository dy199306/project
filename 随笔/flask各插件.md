### Flask-Script

```
1、安装
2、在manager.py中初始化及运行
	manager = Manager(app)
	if __name__ == '__main__':
    manager.run()
```

### flask-blueprint   flask自带，不需要安装

```
1、views.py 创建蓝图对象
	 blue = Blueprint('first',__name__)
2、__init__.py 注册蓝图
from App.views import blue
def createApp():
	app.register.blueprint(blueprint=blue)
```

### Flask-SQLAlchemy

```
1、安装
2、在ext.py中初始化 
db = SQLAlchemy()
def ext_init(app)
	db.init_app(app)
3、在model.py中使用db对象创建模型表
from App.ext import db
  class Province(db.Model):
      id = db.Column(db.Integer, primary_key=True, autoincrement=True)
      name = db.Column(db.String(32), unique=True, nullable=False)
      short_name = db.Column(db.String(8), unique=True, nullable=False)
      capital = db.Column(db.String(32), unique=True, nullable=False)

      # 给Person增加了一个属性 state
      addr = db.relationship('Person', backref='state')
```

### Flask-Migrate

```
1、安装
2、在ext.py中初始化 
db = SQLAlchemy()
def ext_init(app)
	Migrate(app=app,db=db)
3、在manager.py中创建db命令
manager.add_command('db',MigrateCommand)
4、在命令行执行迁移命令(注意执行迁移命令前先导入表名)
python manager.py db init
python manager.py db migrate
python manager.py db upgrade
```

### Flask-Cache 缓存，用来缓存一个视图或者数据

```
1、安装
2、在ext.py中初始化
    cache = Cache(config={'CACHE_TYPE': 'redis'})
    def ext_init(app):
        cache.init_app(app=app)

#redis：使用Redis作为缓存后端
    配置  说明
    CACHE_DEFAULT_TIMEOUT   默认过期/超时时间，单位为秒
    CACHE_KEY_PREFIX    设置cache_key的前缀
    CACHE_REDIS_HOST    redis地址
    CACHE_REDIS_PORT    redis端口
    CACHE_REDIS_PASSWORD    redis密码
    CACHE_REDIS_DB  使用哪个库
    CACHE_REDIS_URL 连接到Redis服务器的URL。示例redis://user:password@localhost:6379/2
    CACHE_ARGS  在缓存类实例化过程中解包和传递的可选列表
    CACHE_OPTIONS   可选字典在缓存类实例化期间传递        
```

```
#缓存一个数据并设置过期时间
cache.set(key,value,timeout=30)  键，值，过期时间：单位秒
cache.set(user.token, user.id, timeout=600)
```

### flask-mail

```
1、安装
2、在settings.py配置
    # Flask-mial必须要配置的三个配置项
    # SMPT服务器地址；发件用户名；发件密码
    MAIL_USERNAME = 'dongying_93@163.com'  # 发送者的邮箱名
    MAIL_DEFAULT_SENDER = 'dongying_93@163.com'  # 发送邮件的邮箱名，这个是收信方看的
    MAIL_SERVER = 'smtp.163.com'  # 发件服务器地址
    MAIL_PASSWORD = 'a123321'  # 邮箱的第三方授权码
3、在ext.py中初始化
	mail = Mail()
def init_app(app):
    mail.init_app(app=app)
```

```
发送邮件
    # 3.1 创建一个Message对象
    msg = Message()
    # 设置邮件的主题
    msg.subject = '欢迎注册1024程序员网'
    # 收件人列表,可以写多个群发，一般里面应该填用户注册时传来的邮箱参数
    msg.recipients = ['346075646@qq.com','...']
    # 邮件的内容
    # 获取激活链接;
    # _external=True 获取绝对路径
    # 不认识的参数会变成请求参数
    #请求结果http://192.168.141.130:5000/handleactive/?token=b1867065-ccd8-4845-8308-755afbdcfd59
    active_url = url_for('first.handle_active', _external=True, token=user.token)

    msg.html = render_template('ActivePage.html', username=username, active_url=active_url)
    # 发件人邮箱.可以不配置发件邮箱，但是需要设置 MAIL_DEFAULT_SENDER 配置项
    # msg.sender = 'chris_jiangwei@126.com'
    # 3.2 把Message发送出去
    mail.send(msg)
```

### flask-restful

```
# 1. 一个URI代表一个资源。路径一般使用复数
# 2. 服务器和客户端传递的是JSON数据
# 3. 通过HTTP请求方式来实现状态转换
# 4. 客户端通过返回值的状态码，查看操作的结果
1. 安装  pip install flask-restful
2. 初始化插件
   Api._api_.py
       #1. 创建Api
       api = Api()
       #2. 定义一个函数，初始化api
       def init_api(app):
           api.init_app(app=app)
       #3. 调用add_resource方法添加路径
       api.add_resource(PersonResource,'/persons/')
   app._init_.py
       app = Flask(__name__)
       init_api(app)
3. 使用插件
   PersonResource.py
       # from flask_restufl import Resource
       class PersonResource(Resource):
       	def get(self):
               # 可以在此直接返回字典
               return {'msg':'收到了get请求'}
           def post(self):
               pass
   Api._init_.py
       api.add_resource(PersonResource,'/persons/')
   数据的序列化

```

### celery

    1、安装  pip install celery
    
    2、定义函数，用来在Flask里集成celery.
    # 代码来源: http://flask.pocoo.org/docs/1.0/patterns/celery/
    import time
    from celery import Celery
    from flask_mail import Message
    from App.ext import mail
    from manager import app
    def make_celery(app):
        celery = Celery('celery_util', broker='redis://localhost:6379')
        class ContextTask(celery.Task):
            def __call__(self, *args, **kwargs):
                with app.app_context():
                    return self.run(*args, **kwargs)
        celery.Task = ContextTask
        return celery
    3、调用make_celery方法，创建celery对象
    celery = make_celery(app=app)
    
    4、定义任务
    @celery.task
    def send_mail(subject, recipients, html):
        time.sleep(20)
        print(recipients)
        msg = Message()
        msg.recipients = recipients
        msg.subject = subject
        msg.html = html
        mail.send(msg)
        print('发送邮件成功')
    
        # 还可以调用send_message方法，给定一系列的参数发送邮件
        # mail.send_message(subject=subject, recipients=recipients, html=html)
    
    5、 启动celery的worker
    # celery -A App.celery_util:celery worker --loglevel=info
    #启动成功页面
    [tasks]
      . App.celery_util.send_mail
    
    [2018-08-16 22:58:16,723: INFO/MainProcess] Connected to redis://localhost:6379//
    
    [2018-08-16 22:58:16,762: INFO/MainProcess] mingle: searching for neighbors
    
    [2018-08-16 22:58:17,861: INFO/MainProcess] mingle: all alone
    
    [2018-08-16 22:58:17,927: INFO/MainProcess] celery@dy-vm ready.
             
### Flask-Bootstrap

### Flask-Session





