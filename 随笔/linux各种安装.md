### 1、SSH远程连接(纯净版中已安装)

1、第一次使用需要先安装ssh

**sudo apt install ssh**

2、卸载ssh命令（作为了解）

sudo apt remove sshd

3、启动ssh服务

service sshd start

4、检查ssh是否可用

sudo service sshd status(显示active ruuning表示可用)

5、查看本机ip地址

ifconfig -a

windows远程连接Linux系统：安装 Xshell 
			在 Xshall 上新建连接
			输入Linux系统的IP地址，端口号：22
			在用户身份验证时输入用户名及密码，点击链接即可


### 2、vim安装

**sudo apt install vim**

与vim相关的常用命令
   a、cat：显示文件内容，正序展示

       cat [选项][文件名]
       cat -n filename 显示行号,(空白行也算)
       cat -b filename  显示行号,(空白行不算)
   b、tac：显示文件内容，倒序展示
       tac [选项][文件名]
   c、head：显示文件部分内容，默认显示前十行
       head [选项][文件名]
       head -n filename   显示前n行
   d、tail：显示文件部分内容，默认显示后十行
       tail [选项][文件名]
       tail -n filename   显示后n行
   e、more：显示文件一部分内容，可以翻页查看所有内容
	空格翻页
   f、wc：计算文件内容
       行数  单词数  字符数  文件名

### 3、谷歌输入法安装

https://www.cnblogs.com/SNine/p/7804276.html

### 4、安装java环境---https://www.linuxidc.com/Linux/2015-01/112030.htm

1、进入到/usr/local/目录并创建java目录

cd /usr/local

sudo mkdir java

2、把jdk压缩包移动到linux常用软件安装目录下

mv jdk安装包名 /usr/local/java

3、解压压缩包

tar -zxvf jdk..

4、删除jdk压缩包

sudo rm -rf jdk..

5、配置环境变量

vim ~/.bashrc

在最下面添加下列代码

export JAVA_HOME=/usr/local/java/jdk1.8.0_25  
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH

6、刷新配置信息

source ~/.bashrc

7、检查java是否安装成功
java -version

显示java version "10.0.1"  表示成功

### 5、pip安装

 sudo apt install python-pip  :  python2.7安装

 sudo apt install python3-pip  :  python3.5安装

### 6、安装虚拟环境

sudo pip install virtualenv

### 7、安装Python3.6---https://www.cnblogs.com/yqpy/p/9116590.html

1、准备内容：需要有一个Python3.6 Linux下的源码安装包
2、解压缩安装包
   解压到某一个常用的软件目录中
   tar -zxvf Python-3.6.5.tgz -C ~/software
3、源码安装时，安装包中都会有说明文档  README.
   如果没有说明文档，去官网查找说明文档
4、安装文档安装
   进入到解压完的python-3.6.5中执行一下命令
   ./configure        配置
   ./configure --enable-optimizations

   sudo make      构建
   sudo make install    安装

5、如果执行完make install报错，报出一个zip...的错误，执行以下命令
    sudo apt install zlib*
    sudo make install

6、如果出现gcc文件错误，安装gcc，执行以下命令
    sudo apt install gcc
    sudo apt install gcc-c++
### 8、安装PyCharm

1、准备内容：需要有一个PyCharm在Linux下的安装包
2、解压缩安装包
   解压到某一个常用的软件目录中
   tar -zxvf Pycharm..gz -C ~/software
3、可以在解压完成的目录中测试pycharm是否可用
   cd ~/software/pycharm-2018.1.3
   cd bin
   ./pycharm.sh  (软件的运行文件为...sh)
4、配置pycharm的环境变量   .bashrc
   在 ~/.bashrc 后面加入以下两条目录路径

   export PyCharm=/home/用户名/software/pycharm-2018.1.3
   export PATH=$PyCharm/bin:$PATH
5、刷新配置文件
    source ~/.bashrc
6、当刷新成功后，可以在当前用户下的任意位置启动pycharm
   启动指令： pycharm.sh

激活:https://blog.csdn.net/xiaoming0018/article/details/80346895

### 9、安装MySQL

命令： sudo apt install mysql-server-5.7

注：安装过程中，会提示输入root用户的密码，输入结束后回车；会再次提示一个确认密码，输入后回车，等待安装。
注：密码不要忘记。

安装结束后进入mysql的指令：
   mysql -u root -p
会提示输入密码（输入root用户的密码，成功后进入mysql）
当出现  mysql>  代表成功进入mysql服务器

### 10、安装MongoDB

1、准备内容：需要有一个MongoDB在Linux下的安装包
2、解压缩安装包
   解压到桌面即可
   tar -zxvf mongo....gz
3、将解压完成的的文件中的内容移动到 /usr/local/mongodb 目录下
   sudo mv mongodb...af/  /usr/local/mongodb

   /usr/local/mongodb  : mongodb的安装路径
4、在mongodb目录下创建data目录，在data目录下创建db及log目录 
    目前db的路径为 /usr/local/mongodb/data/db

5、启动mongodb的服务器端
   进入到mongodb的bin目录下，执行以下命令
   sudo ./mongod -dbpath=data/db的路径
   sudo ./mongod -dbpath=/usr/local/mongodb/data/db
 见到port为27017时代表启动成功

6、启动mongodb的客户端 （之前的服务器端不能关闭，重新打开一个终端）
   进入到mongodb的bin目录下，执行以下命令
   ./mongo

### 11、安装redis

1、解压缩安装包
   tar -zxvf redis..gz -C ~/software
2、进入到解压完成的路径下执行make
  cd ~/software/redis-4.0.9
  make   构建

3、make构建指令结束后，执行以下命令
    make test
4、make test 执行结束后安装redis，执行以下命令
   sudo make instal

5、**linux启动redis服务器端**
   进入src路径下
   cd ~/software/redis-4.0.9/src
   ./redis-server 

   **启动redis客户端**
   再开启一个终端进入src路径下
   cd ~/software/redis-4.0.9/src
  执行 ./redis-cli 

**关闭redis服务**

redis-cli -h 127.0.0.1 -p 6379 shutdown

如果上述方式没有成功停止redis，则可以使用终极武器 

kill -9

6、**windows启动redis服务端**

进入redis目录

执行redis-server.exe

windows启动redis客户端

进入redis目录

执行redis-cli.exe

### 12、ftp (用于文件上传下载的服务器)

1、vsftpd 是一个开源免费的ftp服务器软件，在Linux中最受推崇的FTP服务器，特点小巧轻快，安全易用	
2、xftp	是一个开源免费的ftp客户端软件
3、安装服务器
   sudo apt install vsftpd
4、启动服务器
   systemctl start vsftpd 

5、停止服务器
   systemctl stop vsftpd


6、重启服务器
   systemctl restart vsftpd 
7、默认的ftp服务器端只支持下载，不支持上传，如果想要支持上传，需要设置配置文件    /etc/vsftpd


更改配置文件
sudo vim + /etc/vsftpd.conf

   write_enable=YES
   保存并退出
   注：配置文件中的语法格式 ： 属性=值    等号两边一定不能出现空格
8、更改配置文件后，重启ftp服务器
   systemctl restart vsftpd

### 13、github 下载配置 ---https://www.cnblogs.com/chengxs/p/6297659.html

安装git
   sudo apt install git
**1、git：项目管理工具，可以允许单人或多人合作开发。**
**2、配置git，输入用户名邮箱绑定git**
git config --global user.name "git的用户名"
git config --global user.email "git的邮箱"

**3、根据用户名及邮箱生成密钥(该密钥会用在该账号中)****

3.1 ssh-keygen -t rsa -C "git的邮箱"

执行后所有位置回车即可

生成的密钥默认存放在/home/用户名/.ssh 目录下
密钥的文件为 id_rsa.pub/

3.2 打开密钥所在目录

cd /home/dy

3.3 查看隐藏文件

ls -a

3.4 打开密钥文件

cat id_rsa.pub (密钥范围为ssh至邮箱之前，不包含邮箱，复制该密钥。)

**4、网页端登陆github用户**
将密钥复制到该用户的ssh密钥下
用户 -> settings -> SSH&GPG keys -> new ssh key

**5、检测密钥是否可用**

ssh -T git@github.com

见到successfully ....代表成功


**6、测试能否下载成功**

git clone git项目地址
如果可以下载成功，现在git是没有问题的。

### 十、github上传及更新项目

1、需要在网页端创建一个新的仓库(项目)  new respository
2、在要上传的文件目录下执行git init

    git init
将要上传的文件初始化为可以链接git的文件
3、将需要上传到远程仓库的文件写在add后面
(该命令执行的路径为init过的文件路径下)
   git add abc.txt
让系统自动判断添加的文件：
   git add .

4、给上传的文件添加备注信息
   git commit -m "提交的信息"

5、链接远程仓库(项目地址在git上面复制)
 git remote add origin https://github.com/dy199306/-.git

6、将提交的内容同步至github上
   git push -u origin master 
 (如果正常可以提交文件，不要使用强制提及；如果正常提交失败，可以尝试强制提交)
强制提交： git push -u origin +master

#### 标准流程

git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/dy199306/-.git
git push -u origin master

第二次再提交只需执行

git add README.md

git commit -m "first commit"

git push -u origin master