### 关于 = 后面要不要加引号

name = "张三"    name是一个属性(变量)，不需要加引号；“张三”是name属性的值，需要加引号

"name" : '张三'     "name"是一个key，需要加引号；'张三'是value值，需要加引号

host='0.0.0.0',port=7000,debug=True

1、  = 后面的值是int类型，不需要加引号

​	 a = 2

2、 =  后面是一个变量，不需要加引号

​	b = a

3、 = 后面是True或者False,不需要加引号

​	c = True    c = False

4、 = 后面是属性的值，需要加引号

​	d = "flask"

### 关于os.path.join后面的参数路径要不要加/

使用os.path.join方法拼接路径是，它会自动以/拼接两个路径，所以后面的路径参数前不需要加/
MEDIA_ROOT=os.path.join(BASE_DIR,r'static/media')

### 视图给模版传参的参数类型

参数类型应是一个键值对，模版根据传入的key获取相应的value值

return render('student.html',context={imgpath:imgpath})