# 💡 Python3 相关知识
## 1. Python3的格式化输出
- 在Python中，采用的格式化方式和C语言是一致的，用%实现，举例如下：
```python
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```
- `%`运算符就是用来格式化字符串的。在字符串内部，`%s`表示用字符串替换，`%d`表示用整数替换;
- 有几个`%?`占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个`%?`，括号可以省略。
- 常见的占位符：
> | 占位符 |   替换内容   |
> | :----- | :----------: |
> | %d     |     整数     |
> | %f     |    浮点数    |
> | %s     |    字符串    |
> | %x     | 十六进制整数 |
- 格式化整数和浮点数还可以指定是否补0和整数与小数的位数:
```python
print('%2d-%02d' % (3, 1))
print('%.2f' % 3.1415926)

# 输出,.表示空格
# .3-01
# 3，14
```
- 上面的%2d表示输出的宽度为2，右对齐，所以在3前面加一个空格。
- %02d表示输出的宽度为2，0表示不够宽度用0补齐。
- 如果不太确定应该用什么，%s永远起作用，它会把任何数据类型转换为字符串：
```python
>>> 'Age: %s. Gender: %s' % (25, True)
'Age: 25. Gender: True'
```
- 如果字符串里面的%是一个普通字符，这个时候就需要转义，用%%来表示一个%：
```python
>>> 'growth rate: %d %%' % 7
'growth rate: 7 %'
```
- 另一种格式化字符串的方法是使用字符串的format()方法，它会用传入的参数依次替换字符串内的占位符{0}、{1}:
```python
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```
## 2. Python3的With语句

- 本栏将总结`with`语句的知识点及原理

### 2.1 with与文件相关

- 在读写文件的时候，需要加上**异常处理**操作，如下代码所示：

```python
try:
    fileReader = open('students.txt', 'r')

    for row in fileReader:
        print(row.strip())
except:
    print('Read file failed')
finally:
    fileReader.close()
```

- 上述**异常处理**操作非常繁琐，上述可以使用with语句来代替。
- `with`语句适用于对资源进行访问的场合，**确保不管使用过程中是否发生异常都会执行必要的“清理”操作，释放资源，比如文件使用后自动关闭、线程中锁的自动获取和释放等**。
- 比如上面的代码，通过使用`with`语句改造，就变成了下面这个样子：

```python
with open('students.txt', 'r') as fileReader:
    for row in fileReader:
        print(row.strip())
```

### 2.2 with语句原理

- 要搞清楚`with`语句的原理，先要看一下这几个概念：
  - **上下文管理协议（Context Management Protocol）**：包含方法 `__enter__()`和`__exit__()`，支持该协议的对象要实现这两个方法。
  - **上下文管理器（Context Manager）**：支持上下文管理协议的对象，这种对象实现了`__enter__()`和`__exit__()`方法。上下文管理器定义执行`with`语句时要建立的运行时上下文，负责执行`with`语句块上下文中的进入与退出操作。通常使用`with`语句调用上下文管理器，也可以通过直接调用其方法来使用。
- 一段基本的with语句块如下所示：

```python
with EXPR as VAR:
    BLOCK
```

- 其中**EXPR可以是任意表达式；as VAR是可选的**。其一般的执行过程是这样的：

  - **执行EXPR**，生成上下文管理器context_manager；
  - **获取上下文管理器的`__exit()__`方法**，并保存起来用于之后的调用；
  - **调用上下文管理器的`__enter__()`方法**；如果使用了`as`子句，则将`__enter__()`方法的返回值赋值给`as`子句中的VAR；
  - **执行BLOCK中的表达式；**
  - 不管是否执行过程中是否发生了异常，执行上下文管理器的`__exit__()`方法，`__exit__()`方法负责执行“清理”工作，如释放资源等。如果执行过程中没有出现异常，或者语句体中执行了语句`break/continue/return`，则以`None`作为参数调用`__exit__(None, None, None)`；如果执行过程中出现异常，则使用`sys.exc_info`得到的异常信息为参数调用`__exit__(exc_type, exc_value, exc_traceback)`；
  - 出现异常时，如果`__exit__(type, value, traceback)`返回False，则会重新抛出异常，让`with`之外的语句逻辑来处理异常，这也是通用做法；如果返回True，则忽略异常，不再对异常进行处理。

- **自定义实现上下文管理器**

  学完了`with`语句的内在原理，接下来我们就可以按照这个原理，实现我们自己的上下文管理器。自定义的上下文管理器要实现上下文管理协议所需要的`__enter__()`和`__exit__()`两个方法：

```python
class DBManager(object):
    def __init__(self):
        pass

    def __enter__(self):
        print('__enter__')
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('__exit__')
        return True

def getInstance():
        return DBManager()

with getInstance() as dbManagerIns:
    print('with demo')
```

- 代码运行结果

```python
__enter__
with demo
__exit__
```

## 3. __all__的用法

- python模块中的__all__，用于模块导入时限制，如：`from module import *`
- 此时被导入模块若定义了__all__属性，则只有__all__内指定的属性、方法、类可被导入；若没定义，则导入模块内的所有公有属性，方法和类。
- 实例1：
```python
#bb.py
class A():
    def __init__(self,name,age):
        self.name=name
        self.age=age
class B():
    def __init__(self,name,id):
        self.name=name
        self.id=id
def fun():
    print "func() is run!"
def fun1():
    print "func1() is run!"


#test_bb.py
from bb import *
a=A('zhansan','18')
print a.name,a.age
b=B("lisi",1001)
print b.name,b.id
fun()
fun1()
#输出结果：
zhansan 18
lisi 1001
func() is run!
func1() is run!
```
- 注：由于bb.py中没有定义__all__属性，所以导入了bb.py中所有的公有属性

- 实例2：
```python
#bb.py
__all__=('A','func')
class A():
    def __init__(self,name,age):
        self.name=name
        self.age=age
class B():
    def __init__(self,name,id):
        self.name=name
        self.id=id
def func():
    print "func() is run!"
def func1():
    print "func1() is run!"
#test_bb.py
from bb import *
a=A('zhansan','18')
print a.name,a.age
func()
#b=B("lisi",1001)
#NameError: name 'B' is not defined
#func1()
#NameError: name 'func1' is not defined 　
#运行结果：
zhansan 18
func() is run!
```
- 注：由于bb.py中使用了__all__=('A','func')，所以在别的模块导入该模块时，只能导入__all__中的变量、方法、类

- 实例3：
```python
#bb.py
def func(): #模块中的public方法
    print 'func() is run!'
def _func(): #模块中的protected方法
    print '_func() is run!'
def __func(): #模块中的private方法 
    print '__func() is run!'

#test_bb.py
from bb import *  
#此方式只能导入公有的属性、方法、类【无法导入以单下划线开头（protected）或以双下划线开头(private)的属性、方法、类】
func()
#_func()
#__func()

#运行结果：
func() is run!
```
- 实例4：
```python
#bb.py
__all__=('func','__func','_A')#放入__all__中所有属性均可导入，即使是以下划线开头
class _A():
    def __init__(self,name):
        self.name=name
def func():
    print "func() is run!"
def func1():
    print "func1() is run!"
def _func():
    print "_func() is run!"
def __func():
    print "__func() is run!"


#test_bb.py
from bb import *
func()
#func1()#func1不在__all__中，无法导入 NameError: name 'func1' is not defined 
#_func()#_func不在__all__中，无法导入 NameError: name '_func' is not defined
__func()#__func在__all__中，可以导入
a=_A('zhangsan')#_A在__all__中，可以导入
print a.name


#运行结果：
func() is run!
__func() is run!
zhangsan
```
- 放入__all__中所有属性均可导入，即使是以下划线开头

## 4.  glob的使用
- glob可以查找符合特定规则的文件路径名，使用该模块查找文件，只需要用到： “\*”, “?”, “[]”这三个匹配符
    - ”\*”匹配0个或多个字符；
    - ”?”匹配单个字符；
    - ”[]”匹配指定范围内的字符，如：[0-9]匹配数字.
- glob.glob用法：返回所有匹配的文件路径列表。它只有一个参数pathname，定义了文件路径匹配规则，这里可以是绝对路径，也可以是相对路径。
- 下面是使用glob.glob的例子：
```python
for xmlPath in glob.glob('/media/ai1/DATAPART11/LIDC-IDRI' +"/*"):
#解释：遍历指定文件夹下所有文件或文件夹
 
for xmlPath in glob.glob(xmlPath + "/*/*"):
#解释：遍历指定文件夹下的所有文件夹里的所有文件，/*/*可以根据文件夹层数自主设定
  
img_path = sorted(glob.glob(os.path.join(images, '*.npy')))
#解释：遍历文件夹下所有npy文件,存到一个list中。sorted方法可对列表进行排序，文件名按照字典排序。
 
import glob
#获取指定目录下的所有图片
print glob.glob(r"E:/Picture/*/*.jpg")
#获取上级目录的所有.py文件
print glob.glob(r'../*.py') #相对路径
```

## 5. assert的用法
- python的断言机制，一般的用法是：`assert condition`
- 等效于：
`if not condition:
   raise AssertionError()`
- assert可用于程序的初识阶段，判断某先决条件，若不满足，就不需要进行下去，跳出异常。

## 6. zip的用法
- 用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象
- 在 Python 3.x 中为了减少内存，zip() 返回的是一个对象。如需展示列表，需手动 list() 转换。
- 如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用 * 号操作符，可以将元组解压为列表。
- 实例：
```python
>>>a = [1,2,3]
>>> b = [4,5,6]
>>> c = [4,5,6,7,8]
>>> zipped = zip(a,b)           # 返回一个对象
>>> zipped
<zip object at 0x103abc288>
>>> list(zipped)                # list() 转换为列表
[(1, 4), (2, 5), (3, 6)]
>>> list(zip(a,c))              # 元素个数与最短的列表一致
[(1, 4), (2, 5), (3, 6)]
 
>>> a1, a2 = zip(*zip(a,b))     # 与 zip 相反，zip(*) 可理解为解压，返回二维矩阵式
>>> list(a1)
[1, 2, 3]
>>> list(a2)
[4, 5, 6]
>>>
```
- 遍历：
```python
a = [1,2,3]
b = [4,5,6]
for num_1, num_2 in zip(a,b):
    print(num_1)
    print(num_2)
# 输出
1
4
2
5
3
6
```

## 7.python3的gc模块
- Python中，主要依靠gc（garbage collector）模块的引用计数技术来进行垃圾回收。
- 所谓引用计数，就是考虑到Python中变量的本质不是内存中一块存储数据的区域，而是对一块内存数据区域的引用。
- 所以python可以给所有的对象（内存中的区域）维护一个引用计数的属性。可以用`sys.getrefcount(名称)`来查看变量的引用计数。
- 引用计数+1的情况如下：
    - 对象被创建，例如a=23
    - 对象被引⽤，例如b=a
    - 对象被作为参数，传⼊到⼀个函数中，例如func(a)
    - 对象作为⼀个元素，存储在容器中，例如list1=[a]
- 引用计数-1的情况如下：
    - 对象的别名被显式销毁，例如del a
    - 对象的别名被赋予新的对象，例如a=24
    - ⼀个对象离开它的作⽤域，例如f函数执⾏完毕时，func函数中的局部变量（全局 变量不会）
    - 对象所在的容器被销毁，或从容器中删除对象
- 有三种情况会触发垃圾回收： 1. 调⽤gc.collect(), 2. 当gc模块的计数器达到阀值的时候。 3. 程序退出的时候
- gc.collect() 显式进⾏垃圾回收，可打印出收回垃圾对象的数量。

## 8. UnboundLocalError: local variable 'xxx' referenced before assignment
- 局部变量‘xxx’前边没有定义错误
- 在函数外部定义的变量`xxx`若在函数内部进行修改，则需使用global关键字，否则报错
- 详见[错误例示](https://www.cnblogs.com/kaituorensheng/p/4764078.html)


## 9. list的append和extend方法的区别

- list.append(object) 向列表中添加一个对象object。
- 使用append的时候，是将参数看作一个对象，整体作为一个元素打包添加到指定列表。

- list.extend(iterable) 把一个可迭代对象的内容迭代添加到列表中。
- 使用extend是将一个可迭代对象中的每个元素逐个地添加到列表中，可迭代对象中有几个元素，添加后的列表就比原列表多几个元素。

```python
L1 = [1, 2, 3]
L2 = [1, 2, 3]
a = [4]
L1.append(a)
L2.extend(a)
print(L1)
print(L2)

# 输出
[1, 2, 3, [4]]
[1, 2, 3, 4]
```

## 10. list的insert函数
- insert() 函数用于将指定对象插入列表的指定位置。
- 语法：`list.insert(index, obj)`
```python
aList = [123, 'xyz', 'zara', 'abc']
aList.insert( 3, 2009)
print "Final List : ", aList

# result
Final List : [123, 'xyz', 'zara', 2009, 'abc']
```

## 11. list的[::-1]切片知识点
- b = a[i:j]   表示复制a[i]到a[j-1]，以生成新的list对象
    - 当i缺省时，默认为0，即 a[:3]相当于 a[0:3]
    - 当j缺省时，默认为len(alist), 即a[1:]相当于a[1:10]
    - 当i,j都缺省时，a[:]就相当于完整复制一份a
- b = a[i:j:s]表示：i,j与上面的一样，但s表示步进，缺省为1
    - 所以a[i:j:1]相当于a[i:j]
    - 当s<0时，i缺省时，默认为-1. j缺省时，默认为-len(a)-1
    - 所以a[::-1]相当于 a[-1:-len(a)-1:-1]，也就是从最后一个元素到第一个元素复制一遍，即倒序。
    
## 12. list的pop函数
- pop() 函数用于移除列表中的一个元素（默认最后一个元素），并且返回该元素的值。
- 以下实例展示了 pop()函数的使用方法：
```python
#!/usr/bin/python3
#coding=utf-8
 
list1 = ['Google', 'Runoob', 'Taobao']
list_pop=list1.pop(1)
print "删除的项为 :", list_pop
print "列表现在为 : ", list1

#删除的项为 : Runoob
#列表现在为 :  ['Google', 'Taobao']
```

### 13. Python函数传参中的 \* 与 \*\*
- 在为函数调用时传递参数和函数定义时使用参数的时候，时常会看到有和 \*和 \*\*，下面分别讲解其作用。
- 函数调用时使用\* 和 \*\*:
   - 假设有函数`def test(a, b, c)`
   - `test(*args)` :\* 的作用其实就是把序列args中的每个元素，当作位置参数传进去。比如上面这个代码，如果 args 等于 (1,2,3) ，那么这个代码就等价于 test(1, 2, 3) 。
   - `test(**kwargs)`：\** 的作用则是把字典 kwargs 变成关键字参数传递。比如上面这个代码，如果 kwargs 等于 {‘a’:1,’b’:2,’c’:3} ，那这个代码就等价于 test(a=1,b=2,c=3) 。
- 函数定义时使用 \*和 \*\*:
   - `def test(*args):` 在这里 \*args 表示把传进来的位置参数都装在元组 args 里面。比如说上面这个函数，传参调用test(1, 2, 3)的话， args 的值就是 (1, 2, 3) 
   - `def test(**kwargs): ` \*\* 就是针对关键字参数和字典的了。 传参调用 test(a=1,b=2,c=3)的话， kwargs 的值就是 {'a':1,'b':2,'c':3} 了。
