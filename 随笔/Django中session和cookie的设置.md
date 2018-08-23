#### 1、主页  

某某用户你好  或   登录  注册  

#### 2、登录提交页面  判断请求post/get  登录成功设置cookie

     def login(request):
        if request.method == 'GET':
            return render(request, 'login.html')
        elif request.method == 'POST':
            username = request.POST.get('username')
            userInfos = Ussss.objects.filter(u_name=username)
            if userInfos.exists():  #为什么上面加了.first后这里就不能.exists
                userInfo = userInfos.first()
                password = request.POST.get('password')
                if password == userInfo.u_passwd:
                    response = HttpResponseRedirect(reverse('app:doindex'))#登录成功重定向跳转主页面
                    response.set_cookie('username', username) # 将 username 存入到cookie 中
                    return response
            return HttpResponse("用户名或者密码错误")

#### 3、cookie和session的设置与根据cookie和session取值

**给username设置cookie和session**

    def do_login(request):
        # 将 username 存入到cookie 中
        # 先获取到 username
        username = request.POST.get("username")
        # 存入一个cookie, 存入的是键值对
        response = HttpResponse("提交成功")
        # response.set_cookie("username", username)  #存储到cookie中
        request.session["username"] = username  # 存储session值,注意存储到request中
        return  response

**根据cookie和session取username的值**

    def indexPage(request):
        # cookie根据key取值
         username = request.COOKIES.get("username")
        
        # session根据key取值
        username = request.session.get("username")
        
    	data = {
        	"username":username
    	}
    
    	return  render(request,"index.html",context=data)



#### 4、删除session

    def logout(request):
        方法一：不推荐
        删除客户端的sessionid
        response = HttpResponse("删除成功")
        response.delete_cookie("sessionid")
    
        方法二：删除session数据
        del request.session["username"]
    
        方法三：推荐
         request.session.flush()
         return  HttpResponse("删除成功")
#### 5、对密码进行md5加密

    def doregister(request):
        password = request.POST.get("password")
        #创建一个md5对象
        md5  = hashlib.md5()
        #将一个数据进行MD5处理
    	#需要一个二进制数据,转换成一个128位二进制数据,数据库中存储的是32位的十六机制
        md5.update(password.encode("utf-8"))
        #获取十六进制的数
        passwordMd5 = md5.hexdigest()
    

#### 6、 生成一个token

    def getToken():
    # 规则: 时间戳 + 随机数
    strToken = str(time.time()) + str(random.randint(100000,999999))
    #进行md5信息摘要, "加密"
    # Python中自带md5 库
    #创建一个md5对象
    MD5 = hashlib.md5()
    #将一个数据进行MD5处理
    #需要一个二进制数据,转换成一个128位二进制数据,数据库中存储的是32位的十六机制
    MD5.update(strToken.encode("utf-8"))
    #获取十六进制的数
    token = MD5.hexdigest()
    #返回:
    return token

7、设置与获取token

**设置**

           if userInfos.exists():
               #验证密码
               userinfo = userInfos.first()
               password = request.POST.get("password")
               if password == userinfo.u_passwd:
    #                登录成功
    
    #                 单点登录
                    #登录成功之后,换上新的token 即可
                    userinfo.u_token = getToken()
                    userinfo.save()
    
    #                  设置上token
    
                    # response =  HttpResponse("登录成功")
    
                    response = HttpResponseRedirect(reverse("app:userinfo"))
                    response.set_cookie("token",userinfo.u_token)
                    return response
**获取**

    #主页面
    def get_userinfo(request):
    # 根据token来获取数据
    token = request.COOKIES.get("token",-1)
    userInfo = None
    if token != -1:
        # 根据token查到对应的user
        userInfo =UserInfo.objects.filter(u_token=token).first()
    data = {
        "userinfo":userInfo
    }
    
    return  render(request,"userinfo.html",context=data)








