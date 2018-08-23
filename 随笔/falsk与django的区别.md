### 向模版传参

```
flask：
	return render_template('ActivePage.html', username=username, active_url=active_url)
django：
  data = {
            "user": user,
            "imgPath": imgPath,
        }
	return render(request,'mine/mine.html',context=data)
```

### 模型设计及设置表名

```
flask：
#使用SQLAlchemy的db对象创建模型，flask中外键关联需指定 表名.字段名，被关联的字段必须设置唯一性为True
class Student(db.Model): 
	__tablename__ = 'movies'  #设置表名
	name = db.Column(db.String(16),unique=True,nullable=False)
	class = db.Column(db.String(16),db.ForeignKey(UserTwo.gender),nullable=False)
	
django
#模型设计和设置表名 内置ORM模型,直接继承models.Model,外键关联直接关联表名
from django.db import models
class OrderGoodsModel(models.Model):
  	name = models.CharField(max_length=100)
    og_num = models.IntegerField(default=1)      
    og_goods = models.ForeignKey(MarketGoods)   # 商品
    class Meta:
        db_table = "axf_ordergoodsmodel"
```

