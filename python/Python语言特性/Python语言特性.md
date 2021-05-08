# Python语言特性
Python的函数参数传递
  * 可变和不可变对象
    * 不可变对象：新建后不可改变的对象，只能重新赋值
      * boo/int/float/tuple/str/frozenst
    * 可变对象 : 新建后对象的值可以改变的对象
      * list/set/dict
    * 如果两个变量同时指向一个地址：
      * 可变对象：变量A会随着另一个变量变化而变化，内存地址不变
      * 不可变对象：变量A不会随着另一个变量变化而变化，内存地址发生改变
  * 当作为函数参数时：
    * Python函数参数对于可变对象，函数内对参数的改变会影响到原始对象
    * 不可变对象，改变的是函数内变量的指向对象。
  * 当作为函数默认参数时：
    * 函数默认参数一定要设定为不可变参数
    * 可变对象：当不传递参数而使用默认值时，反复调用会影响对象的值。这是因为函数在定义时就会新建默认 参数对象，之后的调用都是反复调用同一个对象
    *

  * 区别：本质的赋值都是值地址的一个引用，指向的是内存地址。




Python自省
自省： 自省就是面向对象的语言所写的程序在运行时,所能知道对象的类型
  * type()：返回对象的类型
  * dir()：如果没有实参，则返回当前本地作用域中的名称列表。如果有实参，它会尝试返回该对象的有效属性列表。
  * getattr() ： 返回对象命名属性的值。name 必须是字符串。如果该字符串是对象的属性之一，则返回该属性的值。
  * hasattr()： 该实参是一个对象和一个字符串。如果字符串是对象的属性之一的名称，则返回 True，否则返回 False。
  * isinstance()：如果参数 object 是参数 classinfo 的实例或者是其 (直接、间接或 虚拟) 子类则返回 True。 如果 object 不是给定类型的对象，函数将总是返回 False。




Python的下划线
  * 单前导下划线：_var
    * 以单个下划线开头的变量或方法仅供内部使用，为私有属性。是一种约定俗成
    * 仍然可以直接访问
    * 使用通配符导入时会自动忽略
  * 单末尾下划线：var_：
    * 用来解决变量和关键词冲突的时候，末尾加下划线做区分
  * 双前导下划线：__var
    * 双下划线前缀会导致Python解释器重写属性名称，以避免子类中的命名冲突
    * 在类中，双下划线开头的属性，会变成_Test__baz
  * 双前导和末尾下划线：__var__：Python保留了有双前导和双末尾下划线的名称，用于特殊用途
  * 单下划线：_ ： 有时候单个独立下划线是用作一个名字，来表示某个变量是临时的或无关紧要的






迭代器和生成器


  * 迭代器
    * 对象实现了一个__iter__方法
    * __iter__方法返回一个定义了__next__方法的迭代器对象
    * [i for i in range(10)]
  * 生成器
    * 生成器 是一个用于创建迭代器的简单而强大的工具。
    * 返回数据时会使用 yield 语句
    * 每次在生成器上调用 next() 时，它会从上次离开的位置恢复执行（它会记住上次执行语句时的所有数据值）
    * (i for i in range(10))




```
# 生成器示例
def reverse(data):
    for index in range(len(data)-1, -1, -1):
        yield data[index]
>>>
>>> for char in reverse('golf'):
    ...print(char)
    ...
    f
    l
    o
    g
```


  * 生成器表达式
    * 类似列表推导式，但外层为圆括号而非方括号
    * (i for i in range(10))




函数参数
  * 默认参数
    * 在定义函数时给参数一个默认值
    * 默认值只计算一次，不要将可变变量作为默认参数
  * *args 可变参数：
    * 在形参前加一个*，可以传入任意个参数，包括零个参数
    * 在函数内部，参数接受的是一个tuple
    * *也可以用于解包

```
>>> a,*b,c = [1,2,.3,4]
>>> a
1
>>> b
[2,3]
>>c
4
```
  * **kwargs 关键字参数
    * 允许你使用没有事先定义的参数名
    * 与*args类似，关键字参数在函数内部自动组装为一个dict

```
>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```




面向切面编程AOP和装饰器
  * AOP
    * AOP，就是面向切面编程，简单的说，就是动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。
    * 而装饰器，能很好的实现AOP的思想
    *

  * 装饰器
    * 在不影响函数原有的代码结构和功能下，为其额外添加一些功能的函数/类对象
    * 特点:
      * 本质是一个Python函数或者类
      * 接受一个函数对象作为参数
      * 返回一个函数或者类对象
    * 应用场景
      * 插入日志
      * 性能测试
      * 事务处理
    * 代码示例

```
import time
def timeit(func):
    def wrapper():
        start = time.clock()
        func()
        end =time.clock()
        print 'used:', end - start
    return wrapper
@timeit
def foo():
    print 'in foo()'
foo()


# 实际等于foo = timeit(foo)
```
  * 内置装饰器
    * staticmethod ： 将类中定义的实例方法变成静态方法
    * classmethod ： 将类中定义的实例方法变成 类方法
    * property ：  将类中定义的实例方法变成属性

```
# 代码示例
class Rabbit(object):
    def __init__(self, name):
        self._name = name
    @staticmethod
    def newRabbit(name):
        return Rabbit(name)
    @classmethod
    def newRabbit2(cls):
        return Rabbit('')
    @property
    def name(self):
        return self._name
    @name.setter
    def name(self, name):
        self._name = name
r = Rabbit("red")
print(r.name)
r.name = "blue"
print(r.name)
```


    * functools.wraps() :用于保留被修饰函数的特殊属性（例如函数名）
  * 装饰器如何代入参数：

```
# 装饰器如何代入参数
def timeit(func):
    # @functools.wraps(func)
    def wrapper(name):
        start = time.clock()
        func(name)
        end = time.clock()
        print('used:', end - start)
    return wrapper


@timeit
def foo(name):
    print(f'my name is {name}')
```


作用域
  * 四种作用域
    * L（Local）局部作用域：最内层，包含局部变量，比如一个函数/方法内部。
    * E（Enclosing）闭包函数外的函数：包含了非局部(non-local)也非全局(non-global)的变量。比如两个嵌套函数，一个函数（或类） A 里面又包含了一个函数 B ，那么对于 B 中的名称来说 A 中的作用域就为 nonlocal。
    * G（Global）全局作用域：当前脚本的最外层，比如当前模块的全局变量。
    * B（Built-in）内建作用域： 包含了内建的变量/关键字等。，最后被搜索
  * 规则顺序：
    * ： L –> E –> G –>gt; B。

```
g_count = 0  # 全局作用域def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```
  * 引入新的作用域：
    * 模块（module）
    * 类（class）
    * 函数（def、lambda）
  * 全局变量和局部变量
    * global：在作用域中声明该参数为全局变量
    * nonlocal ：在闭包函数中声明该参数为闭包函数外围函数的参数（修改嵌套的作用域）




```
def outer():
    num = 10
    def inner():
        nonlocal num   # nonlocal关键字声明
        num = 100
        print(num)
    inner()
    print(num)
outer()


num = 1
def fun1():
    global num  # 需要使用 global 关键字声明
    print(num)
    num = 123
    print(num)
fun1()
print(num)


```


python3
  * python3改进
    * print由关键字变为函数
    * 编码问题，python3不再有Unicode对象，默认str就是unicode
    * 除法问题，Python3除号返回浮点数
    * 类型注解。帮助IDE实现类型检查
    * 优化的super（）方便直接调用父类函数
    * 高级解包操作.a,b,*c = [1,2,3,4,5,6,7]
    * 限定关键词参数
    * Chained exceptions 。Python3重新抛出异常不会丢失栈信息
    * 一切返回迭代器range,zip,map..etc
    * 生成的pyc文件统一放到__pycache__
  * Python3 新增
    * yield from 链接子生成器
    * asynico内置库，async/await原生协程支持异步编程
    * 新的内置库，enum,mock,等
  * 新式类和旧式类
    * 旧式类：
      * 继承时的查找链是从左到右，深度优先的方式进行查找
      * 默认继承type类
    * 新式类
      * 按照MRO的顺序继承
      * 默认继承object类




__new__和__init__区别
  * __new__
    * 调用以创建一个 cls 类的新实例
    * 将所请求实例所属的类作为第一个参数,其余的参数会被传递给对象构造器表达式
    * 用于创建实例对象以及返回新对象实例 (通常是 cls 的实例)
    * 工作在__init__之前
    * 典型的实现会附带适宜的参数使用 super().__new__(cls[, ...])，通过超类的 __new__() 方法来创建一个类的新实例，然后根据需要修改新创建的实例再将其返回。
    * 如果返回了实例或者cls的自雷，则init会发起调用。反之则不会
  * __init__
    * 工作在__new__之后
    * 用于初始化实例对象
    * 方法绑定的是self参数
    * 不返回值
  * 对象是由 __new__() 和 __init__() 协作构造完成的 (由 __new__() 创建，并由 __init__() 定制)




super
  * super：解决在子类中调用父类的某个已经被覆盖的方法或属性
    * 常见用法
      * 一个常见用法是在 __init__() 方法中确保父类被正确的初始化了
      * 出现在覆盖Python特殊方法的代码中
    * 代码示例

```
class A:
    def spam(self):
        print('A.spam')




class B(A):
    def spam(self):
        print('B.spam')
        super().spam()  # Call parent spam()




class Proxy:
    def __init__(self, obj):
        self._obj = obj


    # Delegate attribute lookup to internal obj
    def __getattr__(self, name):
        return getattr(self._obj, name)




    # Delegate attribute assignment
    def __setattr__(self, name, value):
        if name.startswith('_'):
            super().__setattr__(name, value) # Call original __setattr__
        else:
            setattr(self._obj, name, value)
```


    * 多重继承的情况
      * 对于你定义的每一个类，Python会计算出一个所谓的方法解析顺序(MRO)列表
      * Python会在MRO列表上从左到右开始查找基类，直到找到第一个匹配这个属性的类为止
      * MRO：C3线性化算法来实现
        * 子类会先于父类被检查
        * 多个父类会根据它们在列表中的顺序被检查
        * 如果对下一个类存在两个合法的选择，选择第一个父类




单例模式
  * 单例模式： 单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。
  * 大白话：一个类反复创建的实例是同一个实例
  * 实现方法：
    * 使用__new__实现
    * 使用类装饰器实现
    * 类装饰器 （不易记忆，不推荐）
    * import方法，作为python的模块是天然的单例模式

```
# 使用new
class Borg(object):
    def __new__(cls, *args, **kw):
        if not hasattr(cls, '_instance'):
            cls._instance = super(Borg, cls).__new__(cls, *args, **kw)
        return cls._instance


class MyClass2(Borg):
    a = 1


```






```
#函数装饰器
def singleton(cls):
    instances = {}
    def getinstance(*args, **kw):
        if cls not in instances:
            instances[cls] = cls(*args, **kw)
        return instances[cls]
    return getinstance
@singleton
class MyClass2:
    pass
```
  *





```
#类装饰器
class Singleton(object):
    def __init__(self, cls):
        self._cls = cls
        self._instance = {}
    def __call__(self):
        if self._cls not in self._instance:
            self._instance[self._cls] = self._cls()
        return self._instance[self._cls]


@Singletonclass Cls2(object):
    def __init__(self):
        pass


```




GIL线程全局锁
  * 在Cpython的编译环境中，由于GIL的存在，python无法实现真正的多线程。
    * 如何影响：每个线程需要拿到GIL后才能运行
    * 影响哪些情况：
      * 计算密集型应用
      * CPython编码的运行环境（在此环境中，需要将代码编译成字节码， CPython 字节码的执行，GIL 会导致性能瓶颈
    * GIL动机：CPython 内存管理不是线程安全的，因此需要 GIL 来保证多个原生线程不会并发执行 Python 字节码
    * 如何解决：
      * 使用其他编译环境，如PyPy，Jython
      * 使用多进程或者协程代替




协程
  * 协程：
    * 由程序自身控制的，单线程在在代码执行流程中进行切换，人为的实现多任务并发，是单个线程内的任务调度技巧
    * 优点：
      * 省去了多线程之间的切换开销，获得了更高的运行效率
      * 规避了多线程之间可能出现的并发导致的锁问题
    * 缺点：
      * 无法运用多核CPU的优势
    * Python的协程
      * 早期 yield send 语句
        * 需要next（）或者coroutine.send(None) 预激
        * var = yield xxxx
          * 暂停并返回函数
          * 接收外部send()方法发送过来的值，重新激活函数，并将这个值赋值给var变量
      * @asyncio.coroutine与yield from
        * @asyncio.coroutine：asyncio模块中的装饰器，用于将一个生成器声明为协程
        * yield from 等待另外一个协程的返回
      * async和await




闭包
  * 闭包:
    * 闭包(closure)是函数式编程的重要的语法结构。闭包也是一种组织代码的结构，它同样提高了代码的可重复使用性。


当一个内嵌函数引用其外部作作用域的变量,我们就会得到一个闭包.
    * 创建一个闭包必须满足以下几点:
      * 必须有一个内嵌函数
      * 内嵌函数必须引用外部函数中的变量
      * 外部函数的返回值必须是内嵌函数






Python里的深拷贝与浅拷贝
  * 直接赋值：其实就是对象的引用（别名）。
  * 浅拷贝(copy)：拷贝父对象，不会拷贝对象的内部的子对象。列表的切片就是浅拷贝
  * 深拷贝(deepcopy)： copy 模块的 deepcopy 方法，完全拷贝了父对象及其子对象。

```
import copya = [1, 2, 3, 4, ['a', 'b']]  #原始对象




b = a  #赋值，传对象的引用c = copy.copy(a)  #对象拷贝，浅拷贝d = copy.deepcopy(a)  #对象拷贝，深拷贝




a.append(5)  #修改对象aa[4].append('c')  #修改对象a中的['a', 'b']数组对象




print 'a = ', aprint 'b = ', bprint 'c = ', cprint 'd = ', d




输出结果：
a =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
b =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
c =  [1, 2, 3, 4, ['a', 'b', 'c']]
d =  [1, 2, 3, 4, ['a', 'b']]
```


Python垃圾回收机制


  * Python GC主要使用
    * 引用计数（reference counting）来跟踪和回收垃圾。
    * 在引用计数的基础上，通过“标记-清除”（mark and sweep）解决容器对象可能产生的循环引用问题
    * 通过“分代回收”（generation collection）以空间换时间的方法提高垃圾回收效率。
  * 引用计数
    * PyObject是每个对象必有的内容，其中ob_refcnt就是做为引用计数。当一个对象有新的引用时，它的ob_refcnt就会增加，当引用它的对象被删除，它的ob_refcnt就会减少.引用计数为0时，该对象生命就结束了。
    * 优点
      * 简单
      * 实时性
    * 缺点:
    * 维护引用计数消耗资源
    * 无法解决循环引用
  * 标记-清除机制
    * 基本思路是先按需分配，等到没有空闲内存的时候从寄存器和程序栈上的引用出发，遍历以对象为节点、以引用为边构成的图，把所有可以访问到的对象打上标记，然后清扫一遍内存空间，把所有没标记的对象释放。
  * 分代技术
    * 分代回收的整体思想是：将系统中的所有内存块根据其存活时间划分为不同的集合，每个集合就成为一个“代”，垃圾收集频率随着“代”的存活时间的增大而减小，存活时间通常利用经过几次垃圾回收来度量。





---
### NOTE ATTRIBUTES
>Created Date: 2021-04-19 10:02:16
>Last Evernote Update Date: 2021-04-25 16:24:24
>author: 一的平方
>source: desktop.win
>source-url: about:blank
>source-application: yinxiang.win32