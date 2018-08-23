```
动态添加及限制类的属性及方法
# __slots__  限制动态添加类的属性或方法的个数及名称
from types import MethodType  #动态添加类的方法需导入MethodType
class Person():
    def run(self):
        print("run")
    pass

per1 = Person()       # 实例化对象
per1.name = "Lily"    # 动态添加name属性

def say(self): 	      # 动态添加say方法
     print("say")     # 先定义say方法，注意定义形参self

#使用MethodType动态添加say方法到per1对象上 
# 对象.方法名 = MethodType(函数名称, 当前实例对象的名称)

per1.saying = MethodType(say, per1)
per1.saying()

# 动态限制类只能添加name及age属性，run方法。
#  __slots__ :使用元组的方式赋值
# 注：__slots__定义的属性只对当前类有效，对它继承的子类无效；
# 如果子类中也定义了__slots__属性，这样定义的属性的个数为子类本身的内容
# 加上父类中定义的内容。
class Student():
    __slots__ = ("name", "age", "run")
    pass
```