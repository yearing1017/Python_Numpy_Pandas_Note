# Python_Note
⏰ 记录不熟悉的、遗忘的Python3知识点

## Python3的格式化输出
- 在Python中，采用的格式化方式和C语言是一致的，用%实现，举例如下：
```python
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```
- `%`运算符就是用来格式化字符串的。在字符串内部，`%s`表示用字符串替换，`%d`表示用整数替换;
- 有几个`%?`占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个`%?`，括号可以省略。
- 
