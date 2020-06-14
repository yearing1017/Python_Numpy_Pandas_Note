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

- 看代码：
```python
# 仿照列表排序
A = np.arange(14,2,-1).reshape((3,4)) # -1表示反向递减一个步长
print(A)
# 输出
[[14 13 12 11]
 [10  9  8  7]
 [ 6  5  4  3]]
 
print(np.sort(A))
# 输出
[[11 12 13 14]
 [ 7  8  9 10]
 [ 3  4  5  6]]
 
# 矩阵转置，或print(A.T)
print(np.transpose(A))
# 输出
[[14 10  6]
 [13  9  5]
 [12  8  4]
 [11  7  3]]
 
# clip函数
print(np.clip(A,5,9))
# 输出
[[9 9 9 9]
 [9 9 8 7]
 [6 5 5 5]]
'''
clip(Array,Array_min,Array_max)

将Array_min<X<Array_max X表示矩阵A中的数，如果满足上述关系，则原数不变。

否则，如果X<Array_min，则将矩阵中X变为Array_min;

如果X>Array_max，则将矩阵中X变为Array_max.
'''
```

## 3. Numpy的索引和切片
- 看代码：
```python
import numpy as np
A = np.arange(3,15)
B = A.reshape(3,4)
print(B)
# B
[[ 3  4  5  6]
 [ 7  8  9 10]
 [11 12 13 14]]
 
print(B[2])
# B[2]
[11 12 13 14]

print(B[0][2]) #B[0][2]和B[0,2]等价，打印都是5

# list切片操作
print(B[1,1:3]) # [8 9] 1:3表示1-2不包含3


for row in B:
    print(row)
# 输出
[3 4 5 6]
[ 7  8  9 10]
[11 12 13 14]

# 如果要打印列，则进行转置即可
for column in B.T:
    print(column)
# 输出
[ 3  7 11]
[ 4  8 12]
[ 5  9 13]
[ 6 10 14]

# 多维转一维
A = np.arange(3,15).reshape((3,4))
# print(A)
print(A.flatten())
# flat是一个迭代器，本身是一个object属性
[ 3  4  5  6  7  8  9 10 11 12 13 14]
```
- 一些常见的切片如下图所示，每种颜色对应结果：
![numpy](https://github.com/yearing1017/Python_Numpy_Pandas_Note/blob/master/images/qiepian.jpg)

## 4. Numpy array合并
- 数组合并
```python
import numpy as np
A = np.array([1,1,1])
B = np.array([2,2,2])
print(np.vstack((A,B)))
# vertical stack 上下合并,对括号的两个整体操作。
# 输出
[[1 1 1]
 [2 2 2]]

print(A.shape,B.shape,C.shape)# 从shape中看出A,B均为拥有3项的数组(数列)
# 输出 (3,) (3,) (2, 3)

# horizontal stack左右合并
D = np.hstack((A,B))
print(D) # [1 1 1 2 2 2]
print(A.shape,B.shape,D.shape) # # (3,) (3,) (6,)
# 对于A,B这种，为数组或数列，无法进行转置，需要借助其他函数进行转置
```

- 数组转置为矩阵
```python
print(A[np.newaxis,:]) # [1 1 1]变为[[1 1 1]]
print(A[np.newaxis,:].shape) # (3,)变为(1, 3)
print(A[:,np.newaxis])
# 输出如下：
[[1]
 [1]
 [1]]
```

- 多个矩阵合并
```python
print(A[:,np.newaxis].shape) # (3,1)

A = A[:,np.newaxis] # 数组转为矩阵
B = B[:,np.newaxis] # 数组转为矩阵
print(A)
# 输出
[[1]
 [1]
 [1]]
print(B)
# 输出
[[2]
 [2]
 [2]]

# axis=0纵向合并
C = np.concatenate((A,B,B,A),axis=0)
print(C)
# 输出
[[1]
 [1]
 [1]
 [2]
 [2]
 [2]
 [2]
 [2]
 [2]
 [1]
 [1]
 [1]]

# axis=1横向合并
C = np.concatenate((A,B),axis=1)
print(C)
# 输出
[[1 2]
 [1 2]
 [1 2]]
 
# concatenate的第二个例子
print("-------------")
a = np.arange(8).reshape(2,4)
b = np.arange(8).reshape(2,4)
print(a)
print(b)
print("-------------")
# 输出
-------------
[[0 1 2 3]
 [4 5 6 7]]
[[0 1 2 3]
 [4 5 6 7]]
-------------

# axis=0多个矩阵纵向合并
c = np.concatenate((a,b),axis=0)
print(c)
# 输出
[[0 1 2 3]
 [4 5 6 7]
 [0 1 2 3]
 [4 5 6 7]]

# axis=1多个矩阵横向合并
c = np.concatenate((a,b),axis=1)
print(c)
# 输出
[[0 1 2 3 0 1 2 3]
 [4 5 6 7 4 5 6 7]]
```

## 5. Numpy copy与=
- =赋值方式会带有关联性
```python
import numpy as np
# `=`赋值方式会带有关联性
a = np.arange(4)
print(a) # [0 1 2 3]

b = a
c = a
d = b
a[0] = 11
print(a) # [11  1  2  3]

print(b) # [11  1  2  3]

print(c) # [11  1  2  3]

print(d) # [11  1  2  3]

print(b is a) # True

print(c is a) # True

print(d is a) # True
```

- copy()赋值方式没有关联性
```python
a = np.arange(4)
print(a) # [0 1 2 3]

b =a.copy() # deep copy
print(b) # [0 1 2 3]

a[3] = 44
print(a) # [ 0  1  2 44]
print(b) # [0 1 2 3]

# 此时a与b已经没有关联
```

## 6. Numpy的广播机制
- numpy数组间的基础运算是一对一，也就是a.shape==b.shape，但是当两者不一样的时候，就会自动触发广播机制，如下例子：
```python
from numpy import array
a = array([[ 0, 0, 0],
           [10,10,10],
           [20,20,20],
           [30,30,30]])
b = array([0,1,2])
print(a+b)

# 输出
[[ 0  1  2]
 [10 11 12]
 [20 21 22]
 [30 31 32]]
```
- 广播机制的原则
   - 两个数组的维度不同时，如果两个数组的后缘维度（trailing dimension，即从末尾开始算起的维度）的轴长度相符，广播会在缺失的维度上进行，例如上面的例子，a的shape为(4,3)，b的shape为(3,)，因为从后面开始算的维度相同，都为3，所以在缺失的维度即0维上进行。
   - 两个数组的维度相同时，如果有一个array的有一个维度=1，如下例：在轴1上进行广播
   ```python
   import numpy as np

   arr1 = np.array([[0, 0, 0],[1, 1, 1],[2, 2, 2], [3, 3, 3]])  #arr1.shape = (4,3)
   arr2 = np.array([[1],[2],[3],[4]])    #arr2.shape = (4, 1)

   arr_sum = arr1 + arr2
   print(arr_sum)

   输出结果如下：
   [[1 1 1]
    [3 3 3]
    [5 5 5]
    [7 7 7]]
   ```
- 一幅图中的例子：
![numpy](https://github.com/yearing1017/Python_Numpy_Pandas_Note/blob/master/images/numpy.jpg)


## 7. PIL的Image转为numpy的array
- 使用Image读取图像并进行转换
```python
   label_name = os.listdir('data/dataset1/annotations_prepped_train')[idx]
   label = Image.open('data/dataset1/annotations_prepped_train/' + label_name)
   label = np.array(label)
   label_arr = label.flatten() #一维化数据
```

## 8.numpy计算MIoU等指标——基于混淆矩阵
- `np.diag`：取矩阵的对角线元素
- `np.nanmean`：用于求含有nan值的数组的平均值
- `np.trace`：求矩阵的迹，即对角线元素之和
- `np.dot`：求两个矩阵的积，若为一维，则为内积；若为二维，则为标准矩阵乘积

- 代码如下：
```python
import numpy as np
class Evaluator(object):
    def __init__(self, num_class):
        self.num_class = num_class
        self.confusion_matrix = np.zeros((self.num_class,)*2)
    # 求像素准确率
    def Pixel_Accuracy(self):
        Acc = np.diag(self.confusion_matrix).sum() / self.confusion_matrix.sum()
        return Acc
    # 求类别像素准确率
    def Pixel_Accuracy_Class(self):
        Acc = np.diag(self.confusion_matrix) / self.confusion_matrix.sum(axis=1)
        #Acc = np.nanmean(Acc)
        return Acc
    # 求miou
    def Mean_Intersection_over_Union(self):
        MIoU = np.diag(self.confusion_matrix) / (
                    np.sum(self.confusion_matrix, axis=1) + np.sum(self.confusion_matrix, axis=0) -
                    np.diag(self.confusion_matrix))
        MIoU = np.nanmean(MIoU)
        return MIoU

    def Frequency_Weighted_Intersection_over_Union(self):
        freq = np.sum(self.confusion_matrix, axis=1) / np.sum(self.confusion_matrix)
        iu = np.diag(self.confusion_matrix) / (
                    np.sum(self.confusion_matrix, axis=1) + np.sum(self.confusion_matrix, axis=0) -
                    np.diag(self.confusion_matrix))

        FWIoU = (freq[freq > 0] * iu[freq > 0]).sum()
        return FWIoU
    # 求kappa系数
    def kappa(self):
        pe_rows = np.sum(self.confusion_matrix, axis=0)
        pe_cols = np.sum(self.confusion_matrix, axis=1)
        sum_total = sum(pe_cols)
        pe = np.dot(pe_rows, pe_cols) / float(sum_total ** 2)
        # np.trace求矩阵的迹，即对角线的和
        po = np.trace(self.confusion_matrix) / float(sum_total)
        return (po - pe) / (1 - pe)
    # 生成混淆矩阵
    def _generate_matrix(self, gt_image, pre_image):
        mask = (gt_image >= 0) & (gt_image < self.num_class)
        label = self.num_class * gt_image[mask].astype('int') + pre_image[mask]
        count = np.bincount(label, minlength=self.num_class**2)
        confusion_matrix = count.reshape(self.num_class, self.num_class)
        return confusion_matrix
    # 混淆矩阵相加 
    def add_batch(self, gt_image, pre_image):
        assert gt_image.shape == pre_image.shape
        self.confusion_matrix += self._generate_matrix(gt_image, pre_image)
    # 重置
    def reset(self):
        self.confusion_matrix = np.zeros((self.num_class,) * 2)

```

### 9. numpy中(4,)和(4,1)的区别
- 代码：
```python
>>>import numpy as np
>>>a=np.array([0,1,2,3])
>>>b=np.array([[0],[1],[2],[3]])
>>>c=np.array([[0,1,2,3]])
>>>a.shape
(4,)
>>>b.shape
(4, 1)
>>>c.shape
(1, 4)
```
- a.shape说明数组a的维数是1，其中有4个元素
- b.shape说明数组b的维数是2，每行1个元素，有4行
- c.shape说明数组c的维数是2，每行4个元素，有1行

### 10. Python函数传参中的 \* 与 \*\*
- 在为函数调用时传递参数和函数定义时使用参数的时候，时常会看到有和 \*和 \*\*，下面分别讲解其作用。
- 假设
