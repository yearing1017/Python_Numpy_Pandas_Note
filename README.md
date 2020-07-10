# Python_Numpy_Pandas_Note
⏰ 记录遗忘的Python3、numpy、pandas等知识点

## 💡 Python3 相关知识
- 笔记链接：[Python3.md](https://github.com/yearing1017/Python_Numpy_Pandas_Note/blob/master/Python3.md)

###  1. Python3的格式化输出
- 回顾了python的格式化输出问题，包括使用占位符和format()两种方式的用法。

###  2. Python3的with语句
- 两个方面复习了with语句的用法：和文件相关异常处理、with语句的原理。

###  3. __all__的用法
- 在导入的时候，使用到的__all__笔记

###  4. glob的用法
- 用它可以查找符合特定规则的文件路径名

###  5. assert的用法
- python的断言机制，先判断某条件，不符合则跳出异常

###  6. zip的用法
- 将两个对象打包成元组，返回由元组组成的列表

### 7. python3的gc模块
- 简单学习了解了gc模块进行垃圾回收，以及引用计数的原理。

### 8. UnboundLocalError: local variable 'xxx' referenced before assignment
- 全局变量及glabal关键字示例使用

### 9. list的append和extend方法的区别
- append()和extend()当参数为一个数字(参数限制为一个)时，效果无区别；但当参数为一个列表时，二者就有区别

### 10. list的insert函数
- 指定插入list某位置一个值

### 11. list中[::-1]切片知识小结
- 掌握形如[:-1]和[::-1]的list切片使用

### 12. python模块和包的导入
- [模块导入细节](https://www.cnblogs.com/f-ck-need-u/p/9955485.html)
- [包的导入细节](https://www.cnblogs.com/f-ck-need-u/p/9961372.html)

### 13. list的pop函数
- pop函数用于取出list的某数，默认为最后一个

### 14. Python函数传参中的 \* 与 \*\*
- 记录了在调用和定义函数时，使用\* 和\*\*的含义

### 15. Python交换两数
- 记录一种简单的交换两数的方式

# 💡 Numpy 相关知识
- 笔记链接：[Numpy.md](https://github.com/yearing1017/Python_Numpy_Pandas_Note/blob/master/Numpy.md)

###  1. 有关`np.argmin/argmax`在axis维度上的认识
- 两个方法的使用、返回的类型、axis维度的认识。

###  2. Numpy对ndarry的一些操作
- 仿照列表排序、排序、转置、clip函数

###  3. Numpy的索引和切片
- 一些常用的切片操作

###  4. Numpy array_矩阵合并
- array的合并、array到矩阵、矩阵的合并

###  5. Numpy copy和=
- 直接=赋值和copy方式的区别

###  6. Numpy的广播机制
- 在数组维度不同时如何计算

###  7. PIL的Image转为numpy的array

###  8. numpy计算MIoU等指标—基于混淆矩阵
- 记录numpy对矩阵的几种操作

###  9. numpy中shape为(4,)和(4,1)的区别
- (4,)表示维数为1，有4个元素，如：a = np.array([0,1,2,3])
- (4,1)表示维数为2，4行1列，如：b = np.array([[0],[1],[2],[3]])

# 💡 Pandas 相关知识
- 笔记链接：[Pandas.md](https://github.com/yearing1017/Python_Numpy_Pandas_Note/blob/master/Pandas.md)
