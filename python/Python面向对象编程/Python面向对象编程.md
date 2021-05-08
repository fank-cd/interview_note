# Python面向对象编程
Python中的元类(metaclass)
  * 前置知识


    * 类也是对象
      * 这个对象（类）自身拥有创建对象（类实例）的能力， 它的本质仍然是一个对象
      * 类可以赋值给一个变量
      * 类可以被拷贝
      * 类可以增加属性
      * 类可以将作为函数参数进行传递
    * 使用type关键字可以动态地创建类 ,下面两段代码是等价的

```
>>> class MyShinyClass(object):
…       pass


>>> MyShinyClass = type('MyShinyClass', (), {})  # 返回一个类对象   # type(类名, 父类的元组（针对继承的情况，可以为空），包含属性的字典（名称和值）)
>>> print MyShinyClass
<class '__main__.MyShinyClass'>
>>> print MyShinyClass()  #  创建一个该类的实例
<__main__.MyShinyClass object at 0x8997cec>
```


  * 元类
    * 类创建的叫做实例
    * 元类创建的叫做类
    * Python中一切皆对象，元类也是对象
    * 所有对象的.__class__.__class__都是type
  * __metaclass__属性
    * 在创建类时指定创建类的元类
    * 如果没有则寻找父类的metaclass，模块层次中的metaclass。如果找不到，则用内置的type来创建
    * metaclass中存放的是一段可以创建类的代码
    * 元类的目的是为了在创建类时能够自动地改变类
  * 常见用途：
    * ORM




类方法、静态方法、和类方法.


```
class A(object):
    def foo(self,x):
        print "executing foo(%s,%s)"%(self,x)


    @classmethod
    def class_foo(cls,x):
        print "executing class_foo(%s,%s)"%(cls,x)


    @staticmethod
    def static_foo(x):
        print "executing static_foo(%s)"%x


a=A()
```
  * 类方法
    * 方法参数cls绑定类本身
    * 可以借此访问类变量
    * 类和实例都可以调用
    * 使用装饰器@classmethod定义
  * 实例方法：
    * 方法参数self绑定实例本身
    * 只有实例才能调用
  * 静态方法：
    * 方法本身无需传入参数
    * 类和实例都能调用
    * 使用装饰器@static_method定义
    * 不能访问类变量或者实例变量，使用中更类似于“工具”




| 实例方法| 类方法| 静态方法
---|---|---|---
a = A()   （实例）| a.foo(x)| a.class_foo(x)| a.static_foo(x)
A (类)| 不可用| A.class_foo(x)| A.static_foo(x)
类变量和实例变量
  * 类变量
    * 是可在类的所有实例之间共享的值（也就是说，它们不是单独分配给每个实例的）

```
class Test(object):
    num_of_instance = 0   # 类变量
    def __init__(self, name):
        self.name = name   #实例变量
        Test.num_of_instance += 1

if __name__ == '__main__':
    print Test.num_of_instance   # 0
    t1 = Test('jack')
    print Test.num_of_instance   # 1
    t2 = Test('lucy')
    print t1.name , t1.num_of_instance  # jack 2
    print t2.name , t2.num_of_instance  # lucy 2
```
  * 实例变量
    * 实例化之后，每个实例单独拥有的变量

```
class Person:
    name="aaa"
p1=Person()
p2=Person()
p1.name="bbb"print p1.name  # bbbprint p2.name  # aaaprint Person.name  # aaa
```



---
### NOTE ATTRIBUTES
>Created Date: 2021-04-19 10:33:58
>Last Evernote Update Date: 2021-04-19 11:18:50
>author: 一的平方
>source: desktop.win
>source-url: https://github.com/taizilongxu/interview_python
>source-application: yinxiang.win32