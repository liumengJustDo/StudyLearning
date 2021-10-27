# 一：基础语法篇



## 1.关于循环语句的感想

 ```python
 for  i in range(9):
     print(i)
 ```

就此句而言 ， 区别与c语言和Java开发

```c
C:
for(int i=0;i<9;i++)
{
    print("%d",i);   //强调格式
}

java:
  for i in array[1~9]
  {
      system.out.println(i);   //能简化就简化 感觉和python越来越相似
  }
```



## 2.类与对象的建立

```python
class CLanguage :
    # 下面定义了2个类变量   而且在此语句中我们一定要对此进行赋上初值
    name = "C语言中文网"   
    add = "http://c.biancheng.net"
    def __init__(self,name,add):   ##必须要定义一个构造方法
        #下面定义 2 个实例变量
        self.name = name
        self.add = add
        print(name,"网址为：",add)
    # 下面定义了一个say实例方法
    def say(self, content):
        print(content)
# 将该CLanguage对象赋给clanguage变量
clanguage = CLanguage("C语言中文网","http://c.biancheng.net")
```

```python
class Animal:
    name="动物"
    hobby="吃饭"
    def love(self,name,hobby):
        print(name+"喜欢吃"+hobby)
a=Animal()
a.love("狮子","肉")  ##无参构造方法 与java同理 自行变通即可
```

注：感觉这个和c++语言的集成有点类似 例如 私有属性 和 构造函数都可以直接进行定义 但是这里的python的写法更加随意

 而且就我感觉的话其实我们对于对象了解即可 以后更多的是关注与文库所定义的方法，前任栽树后人乘凉式的！！！有点让人感到头疼

# 二：进阶篇

## 1. matplotlib

 初步认识matplotlib的用法 这里简单看一下

```python
import matplotlib.pyplot as plt
%matplotlib inline 
#1.创建画布
plt.figure(figsize=(10,10),dpi=100)
#2.绘制折线图
plt.plot([1,2,3,4,5,6,7],[14,55,16,12,32,54,87])  
#3.显示画像
plt.show()  
```

<img src="C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210909194419889.png" alt="image-20210909194419889" style="zoom: 67%;" />



np.linspace主要用来创建等差数列

```python
x = np.linspace(0, 10, 200)
x
```

<img src="C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210909202629730.png" alt="image-20210909202629730" style="zoom:67%;" />



```python
plt.fill_between(x, 0, y, facecolor='green', alpha=0.3)
```

`x`：第一个参数表示覆盖的区域，我直接复制为x，表示整个x都覆盖
 `0`：表示覆盖的下限
 `y`：表示覆盖的上限是y这个曲线
 `facecolor`：覆盖区域的颜色
 `alpha`：覆盖区域的透明度[0,1],其值越大，表示越不透明

## 2. numpy

> Numpy（Numerical Python）是一个开源的Python科学计算库，用于快速处理任意维度的数组。
> Numpy支持常见的数组和矩阵操作。对于同样的数值计算任务，使用Numpy比直接使用Python要简洁的多。
> Numpy使用ndarray对象来处理多维数组，该对象是一个快速而灵活的大数据容器。

```python
# 生成10名同学，5门功课的数据
>>> score = np.random.randint(40, 100, (10, 5))
```

![image-20210909204701587](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210909204701587.png)

np.logspace(start=开始值，stop=结束值，num=元素个数，base=指定对数的底, endpoint=是否包含结束值) 等比数列一般为10

```python
np.logspace(0, 2, 3)
```

![image-20210909205603753](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210909205603753.png)

这里的start和stop都是次方数  不要认为只有单个的数字

## 3. panda （重要）

1. 2008年WesMcKinney开发出的库
2. 专门用于数据挖掘的开源python库
3. 以Numpy为基础，借力Numpy模块在计算方面性能高的优势
4. 基于matplotlib，能够简便的画图
5. 独特的数据结构




- Pandas中一共有三种数据结构，分别为：Series、DataFrame和MultiIndex（老版本中叫Panel ）。
- 其中Series是一维数据结构，DataFrame是二维的表格型数据结构，MultiIndex是三维的数据结构。





### 3.1 Series

> Series是一个类似于一维数组的数据结构，它能够保存任何类型的数据，比如整数、字符串、浮点数等，主要由一组数据和与之相关的索引两部分构成。

#### 3.1.1 Series的创建

```python
# 导入pandas
import pandas as pd
pd.Series(data=None, index=None, dtype=None)  #必须要传入数据、索引、类型
```

参数：
data：传入的数据，可以是ndarray、list等
index：索引，必须是唯一的，且与数据的长度相等。如果没有传入索引参数，则默认会自动创建一个从0-N的整数索引。
dtype：数据的类型

例子：

```python
import pandas as pd
pd.Series(np.arange(10))  ##这里的arange 是默认人numpy函数
```

结果：

![image-20210912152319229](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210912152319229.png)

### 3.2  DataFrame

- DataFrame是一个类似于二维数组或表格(如excel)的对象，既有行索引，又有列索引
  行索引，表明不同行，横向索引，叫index，0轴，axis=0

  列索引，表名不同列，纵向索引，叫columns，1轴，axis=1

#### 3.2.1 DataFrame的创建

```python
# 导入pandas
import pandas as pd
pd.DataFrame(data=None, index=None, columns=None)
```

参数：
index：行标签。如果没有传入索引参数，则默认会自动创建一个从0-N的整数索引。
columns：列标签。如果没有传入索引参数，则默认会自动创建一个从0-N的整数索引。

例子：

```python
pd.DataFrame(np.random.randn(2,3))  ##正态分布
```

![image-20210912153528054](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210912153528054.png)



```python
# 生成10名同学，5门功课的数据
score = np.random.randint(40, 100, (10, 5))  ##10行五列
score_df = pd.DataFrame(score)
subjects = ["语文", "数学", "英语", "政治", "体育"]
stu = ['同学' + str(i) for i in range(score_df.shape[0])]  ## score_df.shape[0] =10  score_df.shape[1] = 5
data = pd.DataFrame(score, columns=subjects, index=stu)
```

![image-20210912153516154](C:\Users\liumeng\AppData\Roaming\Typora\typora-user-images\image-20210912153516154.png)



#### 3.2.2 DataFrame的属性

> shape

```python
data.shape ##数组的行列数

结果:(10, 5)
```

> index

DataFrame的**行索引**列表

```python
data.index

结果
Index(['同学0', '同学1', '同学2', '同学3', '同学4', '同学5', '同学6', '同学7', '同学8', '同学9'], dtype='object')
```



> columns

DataFrame的**列索引**列表

```python
data.columns
结果:
    Index(['语文', '数学', '英语', '政治', '体育'], dtype='object')
```



> values

直接获取其中array的值

```python
data.values

结果：
array([[92, 55, 78, 50, 50],
[71, 76, 50, 48, 96],
[45, 84, 78, 51, 68],
[81, 91, 56, 54, 76],
[86, 66, 77, 67, 95],
[46, 86, 56, 61, 99],
[46, 95, 44, 46, 56],
[80, 50, 45, 65, 57],
[41, 93, 90, 41, 97],
[65, 83, 57, 57, 40]])
```



> T

转置:

```p
data.T
```



### 3.3  MultiIndex 与Panel

MultiIndex是三维的数据结构;
多级索引（也称层次化索引）是pandas的重要功能，可以在Series、DataFrame对象上拥有2个以及2个以上的索引。

index属性
names:levels的名称
levels：每个level的元组值

#### 3.3.1 multiIndex的创建

```python
arrays = [[1, 1, 2, 2], ['red', 'blue', 'red', 'blue']]
pd.MultiIndex.from_arrays(arrays, names=('number', 'color'))
# 结果
MultiIndex(levels=[[1, 2], ['blue', 'red']],  ##元组值
codes=[[0, 0, 1, 1], [1, 0, 1, 0]],   ## 0代表第一个值 1 代表第二个值
names=['number', 'color'])    ##元组名称
```





#### 3.3.2 Panel

```python
class pandas.Panel (data=None, items=None, major_axis=None, minor_axis=None)
```

作用：存储3维数组的Panel结构

参数：

- data : ndarray或者dataframe

- items : 索引或类似数组的对象，axis=0

- major_axis : 索引或类似数组的对象，axis=1

- minor_axis : 索引或类似数组的对象，axis=2

  

```python
p = pd.Panel(data=np.arange(24).reshape(4,3,2),
items=list('ABCD'),
major_axis=pd.date_range('20130101', periods=3),
minor_axis=['first', 'second'])
# 结果
<class 'pandas.core.panel.Panel'>
Dimensions: 4 (items) x 3 (major_axis) x 2 (minor_axis)
Items axis: A to D
Major_axis axis: 2013-01-01 00:00:00 to 2013-01-03 00:00:00
Minor_axis axis: first to second
```



批： 不熟练此内容的操作 好像最新版本已经将该内容删除了



# 三：数据

目标

- 记忆DataFrame的形状、行列索引名称获取等基本属性

- 应用Series和DataFrame的索引进行切片获取

- 应用sort_index和sort_values实现索引和值的排序

  

为了更好的理解这些基本操作，我们将读取一个真实的股票数据。关于文件操作，后面在介绍，这里只先用一下API

```python
# 读取文件
data = pd.read_csv("./data/stock_day.csv")
# 删除一些列，让数据更简单些，再去做后面的操作
data = data.drop(["ma5","ma10","ma20","v_ma5","v_ma10","v_ma20"], axis=1)
```



这里的文件导入 我还不是很了解。应该是再github上下载 文件 但是具体的引用还是得看看 ， 就比如这个.csv文件 以及后面我所知道的数据集来着。



