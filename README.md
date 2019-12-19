# Python_Numpy_Pandas_Note
⏰ 记录遗忘的Python3、numpy、pandas等知识点

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


# 💡 Numpy 相关知识

## 1.有关`np.argmin/argmax`在axis维度上的认识

- 先看一段有关argmin的代码：
```python
import numpy as np
a = np.random.randint(24, size=[4,2,3,3])

print("a本身为:")
print(a)

a_argmin = np.argmin(a, axis=1)
print("argmin: axis=1")
print(a_argmin)

print("加上None:")
print(a_argmin[:,None,:,:])
```

- 输出结果
```python
a本身为:
[[[[ 3 14  4]
   [ 5 23 12]
   [ 0 22 20]]

  [[12 17 10]
   [ 1 15  1]
   [ 2 18  7]]]


 [[[19  6  5]
   [ 9 22  7]
   [ 4  8 11]]

  [[10  8 12]
   [21  7  4]
   [17 21  8]]]


 [[[ 5 22  0]
   [ 0 13 13]
   [14  5 13]]

  [[11 18  4]
   [ 0 10  7]
   [22  0  9]]]


 [[[15 21  2]
   [13 20 12]
   [14  3  7]]

  [[12 23 22]
   [11  1 21]
   [11 20 14]]]]
argmin: axis=1
[[[0 0 0]
  [1 1 1]
  [0 1 1]]

 [[1 0 0]
  [0 1 1]
  [0 0 1]]

 [[0 1 0]
  [0 1 1]
  [0 1 1]]

 [[1 0 0]
  [1 1 0]
  [1 0 0]]]
加上None:
[[[[0 0 0]
   [1 1 1]
   [0 1 1]]]


 [[[1 0 0]
   [0 1 1]
   [0 0 1]]]


 [[[0 1 0]
   [0 1 1]
   [0 1 1]]]


 [[[1 0 0]
   [1 1 0]
   [1 0 0]]]]
[Finished in 0.2s]
```
- a本身是一个numpy的多维数组，shape:[4, 2, 3, 3]

- 理解：4代表第0维，其中输出a的结果首尾有4个`[`或`]`就代表该维度的数值

- 理解：2代表第1维，其中每3个`[`或`]`的中间就有两块数据，如下：
```python
[[[19  6  5]
   [ 9 22  7]
   [ 4  8 11]]

  [[10  8 12]
   [21  7  4]
   [17 21  8]]]
```
- 理解：3代表第2维和第3维，shape的最后两个数字表示行列。即每两个`[`或`]`包起来的数据：
```python
  [[10  8 12]
   [21  7  4]
   [17 21  8]]
```
- `np.argmin`表示在某维度上比较大小，取最小的下标组成，返回array，shape为原ndarray去掉axis维度后的shape[4, 3, 3]

- 在上面的例子中，axis=1表示第1维，即在每两个3x3的矩阵上下比较大小

- `np.argmax`同理，即取较大的下标返回。

- 在shape中加None，相当于加了一个新的维度，但是没有新的数值插入，仅仅多了表示维度的`[`和`]`

## 2. Numpy对ndarry的一些操作
- 




# 💡 Pandas 相关知识
