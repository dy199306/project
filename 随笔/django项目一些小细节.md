### 图片上传设置

```
STATIC_URL = '/static/'

#静态目录的配置

STATICFILES_DIRS = [os.path.join(BASE_DIR,'static')]

#文件上传的目录配置
#使用os.path.join方法拼接路径是，它会自动以/拼接两个路径，所以后面的路径参数前不需要加/
MEDIA_ROOT=os.path.join(BASE_DIR,r'static/media')

#设置上传图片的表单
<form action="{% url 'app:register' %}" method="post" enctype="multipart/form-data">
    {% csrf_token %}
    用户名: <input type="text" name="username"><br>
    密码: <input type="password" name="password"><br>
    请上传您的头像：<input type="file" name="icon"><br>
    <input type="submit" value="提交"><br>

#视图函数中获取上传的图片的相对路径
imgPath = '/static/media/' + user.icon.url
#把这个图片路径参数传给模版，在模版中展示图片
return render('student.html',context={imgpath:imgpath})
#在模版中展示图片
<img src='{{imgpath}}'>
```

### label for 

```
<label for="username">用户名:</label>  'username'填的input里面相应的id
<input type="text" id="username" placeholder="请输入用户名/邮箱地址/手机号">
```

### 密码加密处理

    def createPwd(password):
    	mysha512 = hashlib.sha512()
    	mysha512.update(password.encode("utf-8"))
    	return  mysha512.hexdigest()
### 模型类继承

```
# 父类
class Home(models.Model):
    img = models.CharField(max_length=200)
    name = models.CharField(max_length=100)
    trackid = models.CharField(max_length=16)
#     定义成抽象的
    class Meta:
        abstract = True
#轮播图模型 继承home类，需要将父类抽象化
class HomeWheel(Home):
#    指定表名
    class Meta:
        db_table = "axf_wheel"
```