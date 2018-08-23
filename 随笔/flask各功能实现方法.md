### 批量添加数据

```
@db_blue.route('/addpersons/')
def add_persons():
    names = ['张三', '李四', '王五', 'jerry', 'tommy', 'chris', 'allen', 'jerry', 'tom', 'jack', 'rose']
    provinces = Province.query.all()
    persons = []
    for name in names:
        person = Person()
        person.name = name
        person.addr = random.choice(provinces).name
        persons.append(person)
    db.session.add_all(persons)
    db.session.commit()
    return '批量添加用户成功'
```

### 分页功能

```
@db_blue.route('/paginateperson/')
def paginate_paerson():
    # 每一页显示多少条数据
    per_page = int(request.args.get('per_page'))
    # 第几页的页码
    page_num = int(request.args.get('page_num'))
    # 调用paginate方法，实现分页操作
    # 调用 paginate 方法返回值结果是个Pagination对象，它不能够被迭代
    # 这个Pagination对象，保存了分页的相关信息
    result = Person.query.paginate(per_page=per_page, page=page_num)
    # items 属性保存了当前分页的数据
    items = result.items
    return render_template('ShowPersons.html', persons=items)
    模版中
      {% for person in persons %}
    <tr>
      <td>{{ person.id }}</td>
      <td>{{ person.name }}</td>
      <td>{{ person.age }}</td>
      <td>{{ person.gender }}</td>
    </tr>
  {% endfor %}
```

### 数据的排序（offset、limit、order_by）

```
@db_blue.route('/orderperson/')
def order_person():
    # order 进行排序，默认是升序排序
    # 跳过10条显示5条，根据年龄降序排序
    # persons = Person.query.offset(10).limit(5).order_by(-Person.age)
    # 如果要先截取数据再排序，需要在排序之前，调用from_self()方法
    # persons = Person.query.offset(10).limit(5).from_self().order_by(-Person.age)
    persons = Person.query.order_by(-Person.age)
    return render_template('ShowPersons.html', persons=persons)
```

### 收藏功能

```
@db_blue.route('/addtofav/')
def add_to_fav():
    user_id = int(request.args.get('user_id') or 1)
    movie_id = int(request.args.get('movie_id') or 1)
    # 查看是否已经添加到收藏了
    # filter 和 filter_by 方法的返回值是一个BaseQuery对象
    result = Favourite.query.filter_by(user_id=user_id).filter_by(movie_id=movie_id)
    # filter_by的结果是BaseQuery,不是一个列表，不能调用len()
    # 方法一:
    # 可以从BaseQuery里拿到所有的数据，
    # result.all()  # 拿到BaseQuery对象里的所有的数据,这个结果是一个列表
    # result.first()  # 拿到BaseQuery对象里的第一个对象
    # result.count()  # 拿到BaseQuery对象里的对象的数量
    # if len(favs):
    # 方法二:
    # 直接调用BaseQuery里的count方法
    if result.count() > 0:
        db.session.delete(result.first())
        db.session.commit()
        return '已经收藏过了，需要取消收藏'
    # 把用户和电影添加到收藏
    fav = Favourite()
    fav.user_id = user_id
    fav.movie_id = movie_id
    db.session.add(fav)
    db.session.commit()
    return '收藏成功了'
```

### 缓存一个页面

```
# 定义一个函数，返回请求的地址和连接的客户端ip地址
def make_key():
    return str(request.url) + str(request.remote_addr)
@db_blue.route('/showpersons/')
@cache.cached(timeout=50, key_prefix=make_key)  #设置缓存里面保存的访问地址前缀
def show_persons():
    print('接收到了请求，加载数据')
    # 拿到页码，如果拿不到，默认加载第一页
    page_num = int(request.args.get('page_num') or 1)
    # 拿到每一页的显示的数量，如果拿到，默认每页显示5条数据
    per_page = int(request.args.get('per_page') or 5)
    # 调用paginate方法，对数据进行分页
    pagination = Person.query.paginate(page=page_num, per_page=per_page)

    # items属性用来获取当前页的数据
    # persons = pagination.items
    return render_template('ShowPersons.html', pagination=pagination)
```

### 缓存一个数据

```
@db_blue.route('/listpersons/')
def list_persons():
    persons = cache.get('persons')
    if not persons:
        print('缓存里没有数据，重新加载数据')
        # 查询数据库
        persons = Person.query.all()
        # 把查询到的数据缓存起来
        cache.set('persons', persons, timeout=30)
    return render_template('ListPersons.html', persons=persons)
```

### 爬虫判断

```
# @db_blue.before_request
@db_blue.before_app_request
def handle_before():
    # 规定：如果同一个IP地址在5秒钟以内访问的次数超过十次，认为是爬虫
    # 1. 先从缓存里获取次数
    count = cache.get(str(request.remote_addr)) or 0
    # 2. 每一次访问都把此数据加1
    count += 1
    # 3. 每增加完成以后，就把count方法到缓存里
    cache.set(key=str(request.remote_addr), value=count, timeout=5)
    # 4. 判断如果count>10,就认为是爬虫，中断请求
    if count >= 10:
        # abort(404)
        return '您访问的评率太快了，歇会儿再访问吧'
    # 可以使用user_agent字段来判断是否是通过浏览器访问的
    if not request.user_agent:
        return '爬虫别爬了'
    print('接收到了请求')
```

