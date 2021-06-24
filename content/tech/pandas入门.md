---
title: "Pandas入门"
date: 2021-06-20T14:03:46+08:00
draft: true
---

pandas它有两种主要的数据结构。

## 数据结构

### Series

是一种类似于一维数组的对象，它由一组**数据**（各种Numpy数据类型）以及一组与之相关的数据标签（即**索引**）组成。

Series的字符串表现形式为：索引在左边，值在右边。如果没有为数据创建索引，它会自动创建一个从0开始的整数型索引。

``` pandas
In [46]: import pandas as pd

In [47]: pd.Series([1,3,4,5])
Out[47]: 
0    1
1    3
2    4
3    5
dtype: int64
```

也可以自己指定索引。

``` python
pd.Series([1,3,6,7], index=['a','d','c','f'])
Out[48]: 
a    1
d    3
c    6
f    7
dtype: int64
```

与Numpy数组相比，可以通过索引的方式选取Series的单个或多个值。

``` python
In [49]: obj = pd.Series([1,3,6,7], index=['a','d','c','f'])

In [50]: obj['d']
Out[50]: 3

In [51]: obj[['a','d']]
Out[51]: 
a    1
d    3
dtype: int64
```

还可以将Series看成一个定长的有序字典，，可以用在许多原本需要字典参数的函数中。

``` python
In [52]: 'd' in obj
Out[52]: True
```

（因此，如果数据被存放在一个Python字典中，可以直接通过字典来创建Series，结果Series的索引就是原字典的键。）

检测缺失值用isnull和isnotnull函数。

``` python
In [53]: pd.isnull(obj)
Out[53]: 
a    False
d    False
c    False
f    False
dtype: bool

In [54]: obj.isnull()
Out[54]: 
a    False
d    False
c    False
f    False
dtype: bool
```

Series对象本身及其索引都有一个name属性，该属性跟pandas其他功能关系非常密切。

``` python
In [55]: obj.name = 'population'

In [56]: obj.index.name = 'state'

In [57]: obj
Out[57]: 
state
a    1
d    3
c    6
f    7
Name: population, dtype: int64
```

其索引也可以通过赋值的方式就地修改。

``` python
In [58]: obj.index = [1,2,3,4]

In [59]: obj
Out[59]: 
1    1
2    3
3    6
4    7
Name: population, dtype: int64
```

### DataFrame

DataFrame是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型。它既有行索引也有列索引。它可以看作是由Series组成的字典（共用同一个索引）。

最常用的构建方式是传入一个由等长列表或Numpy数组组成的字典。

``` python
In [63]: data = {'state': ['Ohio', 'Nevada'], 'year': [2000, 2001], 'pop': [1.3, 1.5]}

In [64]: df = pd.DataFrame(data)

In [65]: df
Out[65]: 
    state  year  pop
0    Ohio  2000  1.3
1  Nevada  2001  1.5
```

如果指定了列序列，则会按指定顺序排列。

``` python
In [66]: pd.DataFrame(data, columns=['year', 'state', 'pop'])
Out[66]: 
   year   state  pop
0  2000    Ohio  1.3
1  2001  Nevada  1.5
```

用类似字典方式可以将DataFrame的列获取为一个Series。此时Series拥有原DataFrame相同的索引，其name属性也已经设置好了。

``` python
In [67]: df['year']
Out[67]: 
0    2000
1    2001
Name: year, dtype: int64
```

行也可以通过位置或名称进行索引。

``` python
In [72]: df.index = ['b','a']

In [73]: df
Out[73]: 
    state  year  pop
b    Ohio  2000  1.3
a  Nevada  2001  1.5

In [74]: df.loc['b']
Out[74]: 
state    Ohio
year     2000
pop       1.3
Name: b, dtype: object
```

可以通过赋值的方式直接修改列。

``` python
In [75]: df['pop'] = 10

In [76]: df
Out[76]: 
    state  year  pop
b    Ohio  2000   10
a  Nevada  2001   10

In [77]: df['year'] = [200, 300]

In [78]: df
Out[78]: 
    state  year  pop
b    Ohio   200   10
a  Nevada   300   10
```

del可以删除列。

``` python
In [79]: del df['pop']

In [80]: df
Out[80]: 
    state  year
b    Ohio   200
a  Nevada   300
```

与Series一样，可以设置DataFrame的index和columns的name属性。

与Series一样，values属性也会以二位ndarray的形式返回数据。

``` python
In [81]: df.values
Out[81]: 
array([['Ohio', 200],
       ['Nevada', 300]], dtype=object)
```

## 基本功能

### 重新索引

pandas对象的一个重要方法reindex的作用是创建一个适应新索引的新对象。

``` python
In [82]: obj = pd.Series([4,3,2,5], index=['b','d','a','c'])

In [83]: obj
Out[83]: 
b    4
d    3
a    2
c    5
dtype: int64

In [84]: obj2 = obj.reindex(['a','b','c','d'])

In [85]: obj2
Out[85]: 
a    2
b    4
c    5
d    3
dtype: int64
```

如果某个索引值不存在，就引入缺失值。可以指定fill_value参数的值。

### 丢弃指定轴上的项

drop方法返回的是一个在指定轴上删除了指定值的新对象。

``` python
In [86]: obj = pd.Series([4,3,2,5], index=['b','d','a','c'])

In [87]: obj
Out[87]: 
b    4
d    3
a    2
c    5
dtype: int64

In [88]: obj2 = obj.drop('c')

In [89]: obj2
Out[89]: 
b    4
d    3
a    2
dtype: int64
```

对于DataFrame，可以删除任意轴上的索引值。

``` python
In [90]: data = pd.DataFrame(np.arange(16).reshape((4, 4)),
    ...:                     index=['Ohio', 'Colorado', 'Utah', 'New York'],
    ...:                     columns=['one', 'two', 'three', 'four'])

In [91]: data
Out[91]: 
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15

In [92]: data.drop(['Colorado', 'Ohio']) # 这个方向是axis = 0
Out[92]: 
          one  two  three  four
Utah        8    9     10    11
New York   12   13     14    15
```

``` python
In [93]: data.drop('two', axis=1)
Out[93]: 
          one  three  four
Ohio        0      2     3
Colorado    4      6     7
Utah        8     10    11
New York   12     14    15
```

### 索引、选取和过滤

Series索引的工作方式类似于Numpy数组，只不过索引值不只是整数。而切片运算与普通Python不同，其末端是包含的。

``` python
In [94]: obj = pd.Series(np.arange(4.), index=['a', 'b', 'c', 'd'])
    ...: obj
Out[94]: 
a    0.0
b    1.0
c    2.0
d    3.0
dtype: float64

In [95]: obj['b':'c']
Out[95]: 
b    1.0
c    2.0
dtype: float64
```

而对DataFrame进行索引其实就是获取一个或多个列。

``` python
In [96]: data = pd.DataFrame(np.arange(16).reshape((4, 4)),
    ...:                     index=['Ohio', 'Colorado', 'Utah', 'New York'],
    ...:                     columns=['one', 'two', 'three', 'four'])
    ...: data
Out[96]: 
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15

In [97]: data['two']
Out[97]: 
Ohio         1
Colorado     5
Utah         9
New York    13
Name: two, dtype: int32

In [98]: data[['three', 'one']]
Out[98]: 
          three  one
Ohio          2    0
Colorado      6    4
Utah         10    8
New York     14   12
```

几个特殊的情况。

``` python
In [99]: data[:2]  # 这索引的是行
Out[99]: 
          one  two  three  four
Ohio        0    1      2     3
Colorado    4    5      6     7
```

``` python
In [100]: data[data['three'] > 5]
Out[100]: 
          one  two  three  four
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
```

``` python
In [101]: data < 5
Out[101]: 
            one    two  three   four
Ohio       True   True   True   True
Colorado   True  False  False  False
Utah      False  False  False  False
New York  False  False  False  False

In [102]: data[data < 5] = 0
     ...: data
Out[102]: 
          one  two  three  four
Ohio        0    0      0     0
Colorado    0    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
```

为了在DataFrame的行上进行标签索引，引入了专门的字段loc，它使你可以通过Numpy式的标记法以及轴标签从DataFrame中选取行和列的子集。

``` python
In [103]: data.loc['Colorado', ['two', 'three']]
Out[103]: 
two      5
three    6
Name: Colorado, dtype: int32
```

``` python
In [104]: data.iloc[2, [3, 0, 1]]  # 用数字代表行和列
Out[104]: 
four    11
one      8
two      9
Name: Utah, dtype: int32
```

``` python
In [105]: data.iloc[2] # 第3行
Out[105]: 
one       8
two       9
three    10
four     11
Name: Utah, dtype: int32
```

``` python
In [106]: data.iloc[[1, 2], [3, 0, 1]]
Out[106]: 
          four  one  two
Colorado     7    0    5
Utah        11    8    9
```

``` python
In [107]: data.loc[:'Utah', 'two']
Out[107]: 
Ohio        0
Colorado    5
Utah        9
Name: two, dtype: int32
```

``` python
In [108]: data.iloc[:, :3][data.three > 5]
Out[108]: 
          one  two  three
Colorado    0    5      6
Utah        8    9     10
New York   12   13     14
```

## 汇总和计算描述统计

pandas对象拥有一组常用的数学和统计方法，用于从Series中提取单个值（如sum或mean）或从DataFrame的行或列中提取一个Series。跟对应的Numpy数组方法对比，它们都是基于没有缺失数据的假设而构建的。

NA值会自动排除，除非整个切片都是NA。可以通过skipna=False选项禁用该功能。

| 方法           | 说明                       |
| -------------- | -------------------------- |
| count          | 非NA值的数量               |
| describe       | 汇总统计                   |
| min、max       | 最小最大值                 |
| argmin、argmax | 最小最大值索引位置（整数） |
| idxmin、idxmax | 最小最大值的索引值         |
| quantile       | 计算样本分位数             |
| sum            | 总和                       |
| mean           | 平均值                     |
| median         | 算术中位数                 |
| mad            | 根据平均值计算平均绝对离差 |
| var            | 方差                       |
| std            | 标准差                     |
| skew           | 偏度（三阶矩）             |
| kurt           | 峰度（四阶矩）             |
| cumsum         | 累计和                     |
| cummin、cummax | 累计最小值和最大值         |
| cumprod        | 累计积                     |
| diff           | 一阶差分                   |
| pct_change     | 百分数变化                 |

## 处理缺失数据

pandas使用浮点值NaN表示浮点和非浮点数组中缺失数据，它只是一个便于被检测出来的标记而已。Python内置的None值也会被当作NaN处理。

### 滤除缺失数据

对于一个Series，dropna返回一个仅含非空数据和索引值的Series。

对于DataFrame，dropna默认丢弃任何含有缺失值的行。

传入`how='all'`将只丢弃全为NA的那些行。

而要用这种方式丢弃列，只需传入`axis=1`即可。

想留下部分观察数据，可以用thresh参数。

### 填充缺失数据

用fillna函数。可以用inplace参数原地修改。可以用字典实现对不同行或列插入不同的值。

## 数据聚合与分组运算



