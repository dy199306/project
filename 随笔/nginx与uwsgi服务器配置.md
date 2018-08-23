## 1、nginx反向代理服务器

正向代理

浏览器--代理服务器--服务器   

反向代理

浏览器--服务器--代理服务器

#### 基本命令

ps -ef | grep nginx     查看nginx服务是否开启

sudo nginx                  启动nginx

sudo nginx -t -c ~/Desktop/nginx.conf    测试指定配置文件是否能启动

sudo nginx -c ~/Desktop/nginx.conf         指定配置文件启动

nginx -s  参数		

stop 	快速关闭		

quit		优雅的关闭		

reload	重启

#### kill方法杀进程

 sudo kill 主进程号 

项目部署配置	

     location /{
     include /etc/nginx/uwsgi_params;
     uwsgi_pass localhost:8010;
     }
     	
     location /static{
     	 alias  /var/project1804/axf/axf1804/static;  #配置静态文件目录
     }

## 2、使用uwsgi服务器配置

##### 输入ip地址直接访问项目主页

url(r'^$', views.home),

 **1.创建配置文件**

   在当前项目下创建uwsgi.ini文件

 **2.配置uwsgi.ini文件**

[uwsgi]

socket=127.0.0.1:8010    #使用nginx连接时使用

http=127.0.0.1:8010     #   配置http为本机,即直接作为web服务器使用

chdir=/home/dy/djiago1804/myjango   #配置工程目录  pwd获取当前项目目录

wsgi-file=projectModel/wsgi.py    # 配置项目中wsgi的目录  找到wsgi.py在哪个目录下

配置进程，线程信息  #
processes=4
threads=2
enable-threads=True
master=True
pidfile=uwsgi.pid
daemonize=uwsgi.log

###  3.启动uwsgi 服务

    uwsgi --ini  uwsgi.ini 
    会生成 uwsgi.log  与 uwsgi.log文件
**4、在网页使用配置的http地址访问项目**

http://127.0.0.1:8010/

## 3、使用nginx代理uwsgi

1、将项目拷贝到nginx服务器目录下

sudo cp -r test01/ /var/project1804/axf/

2、配置nginx.conf文件

     charset utf-8;
     root   /var/project1804/axf;   #项目目录
     #index  hello.html;


     location /{
     include /etc/nginx/uwsgi_params;    #uwsgi_params的绝对路径
     uwsgi_pass localhost:8010;
     }
     	
     location /static{        #配置静态文件目录
     	 alias  /var/project1804/axf/axf1804/static;
     }
### 3、修改项目中的uwsgi.ini（注意修改的是在nginx服务器目录下的项目里的uwsgi.ini配置）

[uwsgi]

使用nginx连接时使用

socket=127.0.0.1:8010

直接作为web服务器使用（把这句注释掉）

http=127.0.0.1:8010

配置工程目录（项目目录更换到相应的nginx服务器目录下）

chdir=/var/project1804/axf/myjango

配置项目的wsgi目录。（相对于项目目录，不用变更）

wsgi-file=projectModel/wsgi.py

配置进程，线程信息

processes=4
threads=2
enable-threads=True
master=True
pidfile=uwsgi.pid
daemonize=uwsgi.log

### 4、重启nginx和uwsgi服务器

sudo nginx -c nginx.con文件目录

uwsgi --ini /var/project1804/axf/myjango/uwsgi.ini 

### 5、在浏览器中输入ip地址访问项目

http://10.31.152.39/