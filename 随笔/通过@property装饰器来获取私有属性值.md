```
# @property  装饰器
# @property: 可以使对象通过点方法访问私有属性。
class Person():
    def __init__(self,age):
        # 私有属性
        self.__age = age
    # 以往要set get方法来设置及获取__age
    '''
    def setAge(self, age):
        if age < 0:
            self.__age = 1
        else:
            self.__age = age
    def getAge(self):
        return self.__age
    '''
    #  @property属性的函数：目前修改或获取私有变量，属性函数是最简单的方法之一，
    # 把它当作装饰器使用   @property
    # 改写属性的get方法，方法名称为私有变量去掉前面的两个下划线
    # # 装饰器名称为： @property
    @property
    def age(self):
        return self.__age
    # 改写属性的set方法，方法名称为私有变量去掉前面的两个下划线
    # 装饰器名称为： @私有变量名.setter
    @age.setter
    def age(self, age):
        if age < 0:
            age = 1
        self.__age = age
per1 = Person(20)
# 相当于调用set方法，现在调用的是加了@age.setter装饰器的方法
per1.age = 99
# 相当于调用get方法，现在调用的是加了@property装饰器的方法
print(per1.age)
```