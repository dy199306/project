### linux常用命令

1、cd  打开并进入目录

2、mkdir 创建目录/文件夹

3、touch 创建一个文件

4、rm 删除文件

```
-f：强制删除文件或目录；
-r或-R：递归处理，将指定目录下的所有文件与子目录一并处理；
-i：删除已有文件或目录之前先询问用户；
```

5、rmdir 删除文件夹

6、rm -rf  遍历删除非空文件夹

7、rm -rf * 删除所在目录下的所有目录和文件

8、pwd：显示当前目录(绝对路径,完整路径)

9、查看命令帮助文档 1：man 命令 2：命令 --help

10、file：显示文件类型        格式：file 文件或目录名

> **-：文件**
> **d：目录**
> **l：链接**

11、ps：显示文件进程

12、kill：杀死进程
   格式：kill 进程值
         kill -9 进程值   强制杀死进程

13、top：显示当前消耗资源最多的进程

14、free：显示当前内存使用情况
    free -h

15、who：显示登陆用户

16、cal：日历

17、route：显示路由列表

18、grep "abc" node.txt   搜索文件中的文本内容

apt search mysql* | grep server  将某一种的搜索结论作为grep的查找范围，需要使用管道，符号为 |

### 获取系统信息

1、lshw：获取硬件信息
2、lscpu：获取cpu信息
3、lsusb：获取usb信息
4、uname：获取系统相关信息

	uname -a : 获取所有信息
5、arch：查看机器的体系结构
6、df：查看磁盘空间
	df -h ：方便查看数据
7、date：查看时间
8、hostname：主机名称
9、ifconfig：可以查看IP地址（获取网络接口参数）   有些电脑系统或版本ipconfig

### 关机或重启命令

1、poweroff: 立即关机
2、shutdown -h now : 立即关机
3、systemctl poweroff: 立即关机
4、shutdown -h +5: 五分钟后关机
5、shutdown -c : 取消定时关机
6、reboot:重启
7、shutdown -r now:重启
8、systemctl reboot:重启

### 常用快捷键

1、ctrl+c : 强制退出,进程终止

2、ctrl+z : 强制退出,进程未终止

3、ctrl+L  : 清屏

4、win+s  :展示工作空间

5、ctrl+alt+L :屏幕锁屏

6、ctrl + h 显示隐藏文件

### 国内下载源

-i  https://pypi.douban.com/simple  

-i [https://mirrors.aliyun.com/pypi/simple/](http://mirrors.aliyun.com/pypi/simple/)

### 迁移文件

mv 文件名 要迁移的目录路径/文件名

sudo mv redis-server  /usr/local/redis-4.0.9/src/redis-server 

### 复制文件

sudo cp -r 文件名 指定目录   sudo cp -r myjango/ /var/project1804/axf/

### 文件重命名

mv  原文件名 ./文件名  mv bb.txt tt.txt

