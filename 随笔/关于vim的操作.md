```
到文件开头 gg
到文件结尾 shift+g
o：在当前行的下面另起一行
O（shift+o)：在当前行的上面另起一行
```

存盘：

:q! :不存盘退出

:e! :放弃修改文件内容，重新载入该文件编辑

:wq ：存盘退出]

### 2、删除文本全部内容

按Esc键切换到命令行模式
:1,$d

输入:.,$d 一回车就全没了，这个需要光标在第一行才可以

.表示当前位置，到$:表示到末尾，d表示delete删除



```
Linux自带有两个文本编辑器：vi和nano。
使用nano编辑文件：
nano 文件名
点击Ctrl-X可以退出编辑，选择是否保存对文件的改动。
使用vi编辑文件：
vi 文件名
vi有两个模式：一个是编辑模式一个是命令模式。点击i可以从命令模式进入编辑模式，在点击esc键可以重新进入命令模式。我们一般进入编辑模式，来进行添加，修改，删除。但是当我们删除和修改的内容过多的时候，我们使用命令行模式，进行修改，这样方便，快捷，而命令行中，最常用到的是x，dd，u，p这四个命令：x:删除当前字符；dd：删除当前行；u:恢复前一步操作；p:复制之前删除的行。
```

在命令行模式的四个命令

d  删除当前字符

dd  删除当前行

u  恢复前一步操作（相当于ctrl+z）

p 复制之前删除的行

## 与vim相关的常用命令

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



