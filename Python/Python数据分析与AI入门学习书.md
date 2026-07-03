# Python数据分析与AI入门学习书（详解版）

> numpy · matplotlib · pandas · opencv · sklearn · torch
> 面向大专IT专业，每段代码都有详细注释，直接上手

---

## 第一章 NumPy —— 数组计算基础

### 1.1 什么是NumPy

NumPy是Python最基础的科学计算库。它的核心是**ndarray（多维数组）**，比Python原生列表快几十倍，因为：
- 底层用C语言实现，运算速度快
- 数组里所有元素类型一致，内存连续，效率高
- 支持向量化运算，不用写循环

**什么时候用NumPy？** 任何涉及大量数值计算的场景——矩阵运算、数据处理、科学计算、机器学习的数据准备。

### 1.2 创建数组

```python
import numpy as np

# ============ 从列表创建 ============
a = np.array([1, 2, 3])                # 一维数组，形状(3,)
print(a)                                # [1 2 3]
print(type(a))                          # numpy.ndarray

b = np.array([[1, 2, 3], [4, 5, 6]])   # 二维数组，形状(2,3)，就像2行3列的表格
print(b)
# [[1 2 3]
#  [4 5 6]]

c = np.array([[1,2],[3,4],[5,6]])      # 3行2列
print(c.shape)                          # (3, 2)

# ============ 指定数据类型 ============
d = np.array([1, 2, 3], dtype=float)   # 浮点型数组
print(d)                                # [1. 2. 3.]

e = np.array([1, 2, 3], dtype=int)     # 整型数组
print(e.dtype)                          # int64

# ============ 快速创建 ============
# zeros：全0数组，常用于初始化
z = np.zeros((3, 4))                   # 3行4列全0
print(z)
# [[0. 0. 0. 0.]
#  [0. 0. 0. 0.]
#  [0. 0. 0. 0.]]

# ones：全1数组
o = np.ones((2, 3))                    # 2行3列全1
print(o)
# [[1. 1. 1.]
#  [1. 1. 1.]]

# eye：单位矩阵（对角线为1，其余0）
I = np.eye(3)                           # 3×3单位矩阵
print(I)
# [[1. 0. 0.]
#  [0. 1. 0.]
#  [0. 0. 1.]]

# full：填充指定值
f = np.full((2, 3), 7)                 # 2行3列全填7
print(f)
# [[7 7 7]
#  [7 7 7]]

# arange：等差序列（类似range，但返回数组）
seq1 = np.arange(0, 10, 2)             # 从0到10，步长2
print(seq1)                             # [0 2 4 6 8]

seq2 = np.arange(5)                    # 从0到4，步长默认1
print(seq2)                             # [0 1 2 3 4]

# linspace：等间距序列（指定起止和个数，常用于画图的x轴）
seq3 = np.linspace(0, 10, 5)           # 从0到10，分成5个点
print(seq3)                             # [ 0.   2.5  5.   7.5 10. ]

# random：随机数
rand_int = np.random.randint(0, 10, size=(3, 3))  # 3×3随机整数，范围0~9
print(rand_int)

rand_float = np.random.rand(3, 3)      # 3×3随机浮点数，范围0~1
print(rand_float)

rand_normal = np.random.randn(3, 3)    # 3×3标准正态分布（均值0，标准差1）
print(rand_normal)

rand_choice = np.random.choice([1,2,3,4,5], size=5)  # 从列表中随机选5个
print(rand_choice)
```

### 1.3 数组属性

```python
a = np.array([[1, 2, 3], [4, 5, 6]])

a.shape      # (2, 3)   → 形状：2行3列
a.ndim       # 2        → 维度数：二维
a.dtype      # int64    → 数据类型：64位整数
a.size       # 6        → 元素总数：2×3=6
a.itemsize   # 8        → 每个元素占8字节
a.T          # 转置，变成(3, 2)

# 改变形状（不改变数据）
a.reshape(3, 2)    # 变成3行2列
a.reshape(6, 1)    # 变成6行1列
a.reshape(-1, 3)   # -1表示自动计算行数，列数固定为3

# 改变数据类型
a.astype(float)    # 整型转浮点型
a.astype(str)      # 转字符串型
```

### 1.4 索引与切片

```python
# ============ 一维数组索引 ============
a = np.array([10, 20, 30, 40, 50])

a[0]         # 10        → 第0个元素
a[-1]        # 50        → 最后一个元素（-1倒数）
a[-2]        # 40        → 倒数第二个

# ============ 一维数组切片 ============
a[1:4]       # [20,30,40] → 第1到第3个（不包含第4）
a[:3]        # [10,20,30] → 前3个
a[2:]        # [30,40,50] → 第2个开始到末尾
a[::2]       # [10,30,50] → 步长2（隔一个取一个）
a[::-1]      # [50,40,30,20,10] → 反转

# ============ 二维数组索引 ============
b = np.array([[1,2,3],[4,5,6],[7,8,9]])

b[0, 1]      # 2         → 第0行第1列
b[2, 2]      # 9         → 第2行第2列
b[-1, -1]    # 9         → 最后一行最后一列

# ============ 二维数组切片 ============
b[0, :]      # [1,2,3]   → 第0行所有列
b[:, 0]      # [1,4,7]   → 所有行第0列
b[1:, :]     # 第1行开始的所有行
b[:2, 1:]    # 前2行，第1列开始

# ============ 条件索引（布尔索引）============
c = np.array([10, 20, 30, 40, 50])
mask = c > 25                           # [False, False, True, True, True]
c[mask]      # [30, 40, 50] → 只取大于25的

# 一步写
c[c > 25]    # [30, 40, 50]

# 修改满足条件的值
c[c > 25] = 100
print(c)     # [10, 20, 100, 100, 100]
```

### 1.5 运算

```python
# ============ 逐元素运算 ============
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

a + b        # [5, 7, 9]     → 逐元素加
a - b        # [-3, -3, -3]  → 逐元素减
a * b        # [4, 10, 18]   → 逐元素乘（不是矩阵乘！）
a / b        # [0.25, 0.4, 0.5] → 逐元素除
a ** 2       # [1, 4, 9]     → 逐元素平方
np.sqrt(a)   # [1., 1.41, 1.73] → 逐元素开方

# ============ 比较运算 ============
a > 2        # [False, False, True]  → 逐元素比较
a == b       # [False, False, False]

# ============ 广播机制 ============
# 形状不同的数组运算时，NumPy会自动"扩展"较小的数组
a = np.array([[1,2,3],[4,5,6]])   # 形状(2,3)
b = np.array([10,20,30])          # 形状(3,)
a + b        # b自动扩展成2行3列再相加
# [[11 22 33]
#  [14 25 36]]

c = np.array([[1],[2]])           # 形状(2,1)
a + c        # c自动扩展成2行3列
# [[2 3 4]
#  [6 7 8]]

# ============ 矩阵运算 ============
A = np.array([[1,2],[3,4]])
B = np.array([[5,6],[7,8]])

A @ B        # 矩阵乘法
# [[19 22]
#  [43 50]]

A.dot(B)     # 同上，矩阵乘法

np.matmul(A, B)  # 同上

A.T          # 矩阵转置

np.linalg.inv(A)  # 矩阵求逆

np.linalg.det(A)  # 矩阵行列式

# ============ 统计函数 ============
a = np.array([1, 2, 3, 4, 5])

np.sum(a)       # 15    → 求和
np.mean(a)      # 3.0   → 平均值
np.median(a)    # 3.0   → 中位数
np.max(a)       # 5     → 最大值
np.min(a)       # 1     → 最小值
np.std(a)       # 标准差
np.var(a)       # 方差
np.cumsum(a)    # [1,3,6,10,15] → 累加
np.cumprod(a)   # [1,2,6,24,120] → 累乘

# 按轴统计（二维）
b = np.array([[1,2,3],[4,5,6]])

np.sum(b, axis=0)   # [5,7,9]   → 每列求和（沿行方向压）
np.sum(b, axis=1)   # [6,15]    → 每行求和（沿列方向压）
np.mean(b, axis=0)  # [2.5,3.5,4.5] → 每列平均

# axis=0 → 沿行压（跨行操作） → 结果是每列的统计
# axis=1 → 沿列压（跨列操作） → 结果是每行的统计
```

### 1.6 形状操作

```python
a = np.arange(12)   # [0,1,2,3,4,5,6,7,8,9,10,11]

# reshape：改变形状，不改变数据
a.reshape(3, 4)     # 变成3行4列
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]]

a.reshape(4, 3)     # 变成4行3列
a.reshape(2, 6)     # 变成2行6列
a.reshape(-1, 3)    # -1=自动计算行数，列=3 → 4行3列

# flatten vs ravel：都展平成一维
a_2d = a.reshape(3, 4)
flat = a_2d.flatten()   # 返回副本（修改flat不影响原数组）
rav = a_2d.ravel()      # 返回视图（修改rav会影响原数组）

# 拼接
x = np.array([[1,2],[3,4]])
y = np.array([[5,6],[7,8]])

np.concatenate([x, y], axis=0)   # 竖向拼接 → (4,2)
np.concatenate([x, y], axis=1)   # 横向拼接 → (2,4)

np.vstack([x, y])                # 竖向堆叠（同axis=0）
np.hstack([x, y])                # 横向堆叠（同axis=1）

np.stack([x, y], axis=0)         # 沿新维度堆叠 → (2,2,2)

# 分割
long = np.arange(12)
np.split(long, 3)                # 分成3段：[0~3],[4~7],[8~11]
np.split(long, [3, 7])           # 在位置3和7切割

# 重复
a = np.array([1, 2, 3])
np.repeat(a, 3)                  # [1,1,1,2,2,2,3,3,3] → 每个元素重复3次
np.tile(a, 3)                    # [1,2,3,1,2,3,1,2,3] → 整段重复3次
```

### 1.7 常用技巧

```python
# ============ 条件替换 ============
a = np.array([1, 2, 3, 4, 5])
np.where(a > 3, 100, 0)    # [0, 0, 0, 100, 100] → 大于3换100，否则换0

# ============ 唯一值 ============
b = np.array([1, 2, 2, 3, 3, 3, 4])
np.unique(b)                 # [1, 2, 3, 4] → 唯一值（已排序）

# ============ 排序 ============
c = np.array([3, 1, 4, 1, 5, 9])
np.sort(c)                   # [1, 1, 3, 4, 5, 9] → 升序排序
np.argsort(c)                # [1, 3, 0, 2, 4, 5] → 排序后的索引

# ============ 随机种子 ============
np.random.seed(42)           # 设置种子，保证每次运行结果一样（实验可复现）
np.random.rand(3)            # 每次都得到相同的随机数

# ============ 文件读写 ============
np.save('data.npy', a)              # 保存单个数组
np.load('data.npy')                 # 加载

np.savez('multi.npz', x=x, y=y)    # 保存多个数组
data = np.load('multi.npz')
data['x']                           # 取出x

np.savetxt('data.txt', a_2d)        # 保存为文本
np.loadtxt('data.txt')              # 加载文本
```

---

## 第二章 Matplotlib —— 画图可视化

### 2.1 什么是Matplotlib

Matplotlib是Python最经典的绘图库。它能画折线图、柱状图、散点图、饼图等几乎所有常见图表。

**核心对象：**
- **Figure**：整个画布（一张图）
- **Axes**：画布上的一个子图区域（可以有多个）
- **Artist**：图上的所有元素（线、点、文字等）

### 2.2 折线图（最基础）

```python
import matplotlib.pyplot as plt
import numpy as np

# ============ 最简单的折线图 ============
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

plt.plot(x, y)                       # 画线
plt.show()                            # 显示

# ============ 加装饰 ============
plt.plot(x, y, 
         color='red',                 # 线颜色：red/blue/green/#FF6600
         linewidth=2,                 # 线宽
         marker='o',                  # 数据点标记：o圆形/s方形/^三角/D菱形/*/x
         markersize=8,                # 标记大小
         linestyle='-',               # 线型：-实线/--虚线/:点线/-.点划线
         label='数据A')               # 图例标签

plt.title('折线图标题', fontsize=14)  # 标题
plt.xlabel('x轴', fontsize=12)        # x轴标签
plt.ylabel('y轴', fontsize=12)        # y轴标签
plt.grid(True, alpha=0.3)             # 网格线，alpha透明度
plt.legend(loc='upper left')          # 图例位置：upper left/right/lower/center/best
plt.xlim(0, 6)                        # x轴范围
plt.ylim(0, 12)                       # y轴范围
plt.savefig('line.png', dpi=150)      # 保存，dpi分辨率
plt.show()
```

### 2.3 多条线

```python
x = np.linspace(0, 2*np.pi, 100)     # 0到2π，100个点

plt.figure(figsize=(10, 6))           # 设置画布大小（宽10，高6英寸）

plt.plot(x, np.sin(x), 'r-', label='sin', linewidth=2)     # 红实线
plt.plot(x, np.cos(x), 'b--', label='cos', linewidth=2)    # 蓝虚线
plt.plot(x, np.sin(2*x), 'g:', label='sin2x', linewidth=1) # 绿点线

plt.legend()                          # 显示图例
plt.title('三角函数')
plt.xlabel('x')
plt.ylabel('y')
plt.grid(True)
plt.savefig('trig.png')
plt.show()

# 简写格式：'颜色+线型+标记'
# 'r-o' → 红色实线圆形标记
# 'b--s' → 蓝色虚线方形标记
# 'g:^' → 绿色点线三角标记
```

### 2.4 柱状图

```python
# ============ 竖向柱状图 ============
names = ['张三', '李四', '王五', '赵六']
scores = [85, 90, 78, 95]
colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#96CEB4']

plt.bar(names, scores, color=colors, width=0.6, edgecolor='gray')
plt.title('成绩柱状图')
plt.xlabel('姓名')
plt.ylabel('成绩')
plt.ylim(0, 100)

# 在柱子上加数字
for i, v in enumerate(scores):
    plt.text(i, v+2, str(v), ha='center', fontsize=12)

plt.show()

# ============ 横向柱状图 ============
plt.barh(names, scores, color=colors)
plt.xlabel('成绩')
plt.show()

# ============ 分组柱状图（对比两组数据）============
men_scores = [80, 85, 90, 75]
women_scores = [88, 92, 79, 98]
x_pos = np.arange(len(names))
bar_width = 0.35

plt.bar(x_pos - bar_width/2, men_scores, bar_width, label='男生', color='#3498db')
plt.bar(x_pos + bar_width/2, women_scores, bar_width, label='女生', color='#e74c3c')
plt.xticks(x_pos, names)    # x轴标签
plt.legend()
plt.show()
```

### 2.5 散点图

```python
# ============ 基本散点图 ============
x = np.random.rand(50)
y = np.random.rand(50)

plt.scatter(x, y, 
            c='red',                    # 颜色
            s=30,                       # 点大小
            alpha=0.5,                  # 透明度
            marker='o',                 # 标记形状
            edgecolors='black')         # 边框颜色
plt.title('散点图')
plt.show()

# ============ 按类别分色散点图 ============
# 生成模拟数据：3个类别
n = 30
x1 = np.random.randn(n) + 1; y1 = np.random.randn(n) + 1
x2 = np.random.randn(n) + 3; y2 = np.random.randn(n) + 3
x3 = np.random.randn(n) + 5; y3 = np.random.randn(n) + 5

plt.scatter(x1, y1, c='red', label='类别1')
plt.scatter(x2, y2, c='green', label='类别2')
plt.scatter(x3, y3, c='blue', label='类别3')
plt.legend()
plt.title('分类散点图')
plt.show()

# ============ 用颜色映射值大小 ============
x = np.random.rand(100)
y = np.random.rand(100)
values = np.random.rand(100)

plt.scatter(x, y, c=values, cmap='viridis', s=50)  # cmap颜色映射
plt.colorbar()    # 显示颜色条
plt.title('颜色映射散点图')
plt.show()
```

### 2.6 直方图

```python
# ============ 基本直方图 ============
data = np.random.randn(1000)           # 1000个正态分布数据

plt.hist(data, 
         bins=30,                       # 分成30个桶
         color='steelblue',            # 颜色
         edgecolor='white',            # 边框颜色
         alpha=0.7,                    # 透明度
         density=False)                # False=频数，True=频率（概率密度）

plt.title('直方图')
plt.xlabel('值')
plt.ylabel('频数')
plt.show()

# ============ 对比两组数据的直方图 ============
data1 = np.random.randn(500) + 1
data2 = np.random.randn(500) + 3

plt.hist(data1, bins=30, alpha=0.5, label='分布1', color='red')
plt.hist(data2, bins=30, alpha=0.5, label='分布2', color='blue')
plt.legend()
plt.show()
```

### 2.7 饼图

```python
sizes = [40, 30, 20, 10]
labels = ['类别A', '类别B', '类别C', '类别D']
colors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#96CEB4']
explode = [0.1, 0, 0, 0]              # 突出类别A（偏移0.1）

plt.pie(sizes, 
        labels=labels, 
        colors=colors, 
        autopct='%1.1f%%',            # 显示百分比
        explode=explode,              # 突出某块
        shadow=True,                  # 阴影效果
        startangle=90)                # 起始角度

plt.title('饼图')
plt.axis('equal')                     # 保证圆形不变形
plt.show()
```

### 2.8 子图布局

```python
# ============ 2×2子图 ============
fig, axes = plt.subplots(2, 2, figsize=(10, 8))   # 2行2列，画布10×8

axes[0,0].plot([1,2,3], [1,4,9], 'r-o')
axes[0,0].set_title('折线图')

axes[0,1].bar(['A','B','C'], [3,5,2], color=['red','green','blue'])
axes[0,1].set_title('柱状图')

axes[1,0].scatter(np.random.rand(30), np.random.rand(30))
axes[1,0].set_title('散点图')

axes[1,1].hist(np.random.randn(100), bins=20, color='steelblue')
axes[1,1].set_title('直方图')

plt.tight_layout()    # 自动调整间距，防止标题重叠
plt.savefig('subplots.png')
plt.show()

# ============ 不规则布局 ============
fig = plt.figure(figsize=(10, 8))

ax1 = fig.add_subplot(2, 2, 1)    # 占第1格
ax2 = fig.add_subplot(2, 2, 2)    # 占第2格
ax3 = fig.add_subplot(2, 1, 2)    # 占下半部分（整行）

ax1.plot([1,2,3], [1,4,9])
ax2.bar(['A','B'], [3,5])
ax3.plot(np.linspace(0,10,100), np.sin(np.linspace(0,10,100)))

plt.tight_layout()
plt.show()
```

### 2.9 样式美化

```python
# ============ 使用预设样式 ============
plt.style.use('seaborn-v0_8')       # 海洋风格，好看
# 其他可选：'ggplot', 'dark_background', 'fivethirtyeight', 'bmh'

plt.plot([1,2,3], [1,4,9])
plt.show()

# ============ 中文字体设置 ============
plt.rcParams['font.sans-serif'] = ['SimHei']    # 黑体（Windows）
plt.rcParams['axes.unicode_minus'] = False       # 正常显示负号

plt.plot([1,2,3], [1,4,9])
plt.title('中文标题')    # 现在能显示中文了
plt.show()
```

---

## 第三章 Pandas —— 表格数据处理

### 3.1 什么是Pandas

Pandas是Python最常用的数据分析库。核心是**DataFrame**——就像Excel表格，有行有列有索引，但比Excel强大得多，能处理百万行数据。

**两个核心对象：**
- **Series**：一列数据（一维）
- **DataFrame**：多列数据组成的表格（二维）

### 3.2 创建DataFrame

```python
import pandas as pd
import numpy as np

# ============ 从字典创建 ============
df = pd.DataFrame({
    '姓名': ['张三', '李四', '王五', '赵六'],
    '年龄': [20, 21, 19, 22],
    '成绩': [85, 90, 78, 95],
    '班级': ['计网1', '计网2', '计网1', '计网3']
})
print(df)
#    姓名  年龄  成绩   班级
# 0  张三  20  85  计网1
# 1  李四  21  90  计网2
# 2  王五  19  78  计网1
# 3  赵六  22  95  计网3

# ============ 从列表创建 ============
data = [
    ['张三', 20, 85],
    ['李四', 21, 90],
    ['王五', 19, 78]
]
df2 = pd.DataFrame(data, columns=['姓名', '年龄', '成绩'])

# ============ 从NumPy数组创建 ============
arr = np.random.randn(5, 3)
df3 = pd.DataFrame(arr, columns=['A', 'B', 'C'])

# ============ 从文件读取 ============
df_csv = pd.read_csv('data.csv')             # CSV文件
df_csv = pd.read_csv('data.csv', encoding='gbk')  # 中文CSV可能需要指定编码
df_excel = pd.read_excel('data.xlsx')        # Excel文件
df_excel = pd.read_excel('data.xlsx', sheet_name='Sheet2')  # 指定工作表
df_txt = pd.read_csv('data.txt', sep='\t')   # txt文件，制表符分隔
```

### 3.3 查看数据

```python
df.head()          # 前5行
df.head(10)        # 前10行
df.tail()          # 后5行
df.tail(3)         # 后3行

df.shape           # (4, 4) → 4行4列
df.columns         # 列名列表：Index(['姓名','年龄','成绩','班级'])
df.index           # 行索引：RangeIndex(start=0, stop=4, step=1)
df.dtypes          # 每列数据类型
df.info()          # 整体概览：行数、列数、每列类型、内存占用

df.describe()      # 数值列统计摘要
#        年龄      成绩
# count  4.00    4.00
# mean   20.50   87.00
# std    1.29    7.55
# min    19.00   78.00
# 25%    19.75   82.25
# 50%    20.50   87.50
# 75%    21.25   91.75
# max    22.00   95.00

df.describe(include='all')  # 包括字符串列的统计

df.values          # 获取底层NumPy数组
```

### 3.4 选列选行

```python
# ============ 选列 ============
df['姓名']                  # 单列 → Series
df[['姓名', '成绩']]        # 多列 → DataFrame
df.姓名                     # 另一种写法（列名无空格时可用）

# ============ 选行 ============
df[0:2]                     # 第0到第1行（切片）
df.loc[0]                   # 按索引标签选第0行
df.loc[0:2]                 # 第0到第2行（含2）

# ============ loc：按标签选 ============
df.loc[0, '姓名']           # 第0行"姓名"列 → '张三'
df.loc[0:2, ['姓名','成绩']]  # 第0~2行，姓名和成绩列
df.loc[:, '成绩']            # 所有行的成绩列

# ============ iloc：按位置选 ============
df.iloc[0]                  # 第0行
df.iloc[0, 1]               # 第0行第1列 → 年龄20
df.iloc[0:2, 0:2]           # 前2行前2列
df.iloc[[0,3], [0,2]]       # 第0和第3行，第0和第2列

# ============ 条件筛选 ============
df[df['成绩'] > 80]                          # 成绩大于80
df[(df['成绩'] > 80) & (df['年龄'] < 21)]    # 多条件（&且 |或）
df[df['班级'].isin(['计网1', '计网3'])]       # 班级在指定列表中
df[df['姓名'].str.contains('张')]             # 名字含"张"

# ============ query方法（更直观）============
df.query('成绩 > 80')
df.query('成绩 > 80 and 年龄 < 21')
```

### 3.5 排序

```python
# ============ 按值排序 ============
df.sort_values('成绩')                      # 成绩升序
df.sort_values('成绩', ascending=False)     # 成绩降序
df.sort_values(['班级', '成绩'], ascending=[True, False])  # 班级升序+成绩降序

# ============ 按索引排序 ============
df.sort_index()                             # 按行索引排序
df.sort_index(axis=1)                       # 按列名排序
```

### 3.6 增删改

```python
# ============ 加列 ============
df['等级'] = ['良', '优', '中', '优']                    # 直接赋值
df['是否及格'] = df['成绩'] >= 60                        # 用表达式
df['成绩排名'] = df['成绩'].rank(ascending=False)       # 排名

# 用函数生成
df['年龄段'] = df['年龄'].apply(lambda x: '少年' if x<20 else '青年')

# ============ 加行 ============
new_row = pd.DataFrame({'姓名':['钱七'], '年龄':[20], '成绩':[88], '班级':['计网2']})
df = pd.concat([df, new_row], ignore_index=True)        # ignore_index重建索引

# 多行追加
df2 = pd.DataFrame({'姓名':['孙八','周九'], '年龄':[23,18], '成绩':[70,82], '班级':['计网1','计网3']})
df = pd.concat([df, df2], ignore_index=True)

# ============ 删列 ============
df.drop('等级', axis=1)                  # 删除"等级"列（返回新DataFrame）
df.drop(columns=['等级', '年龄段'])      # 删除多列
df.drop('等级', axis=1, inplace=True)    # inplace=True直接在原df上改

# ============ 删行 ============
df.drop(0)                               # 删除第0行
df.drop([0, 2])                          # 删除第0和第2行
df.drop(df[df['成绩']<60].index)         # 删除成绩<60的行

# ============ 改值 ============
df.loc[0, '成绩'] = 88                   # 改某个值
df.loc[0:2, '成绩'] = [80, 85, 75]       # 改多个值
df['成绩'] = df['成绩'] + 5              # 全列加5

# ============ 改列名 ============
df.rename(columns={'成绩': '分数', '姓名': '名字'})
df.columns = ['name', 'age', 'score', 'class']  # 全改

# ============ 改索引 ============
df.set_index('姓名')                      # 把"姓名"列设为索引
df.reset_index()                          # 重置索引为默认数字
```

### 3.7 统计与分组

```python
# ============ 单列统计 ============
df['成绩'].mean()         # 87.0   → 平均
df['成绩'].max()          # 95     → 最大
df['成绩'].min()          # 78     → 最小
df['成绩'].std()          # 标准差
df['成绩'].median()       # 中位数
df['成绩'].sum()          # 总和
df['成绩'].count()        # 非空个数

df['班级'].value_counts()             # 频数统计
# 计网1    2
# 计网2    1
# 计网3    1

df['班级'].unique()                   # 唯一值列表
df['班级'].nunique()                  # 唯一值个数

# ============ 分组统计 ============
df.groupby('班级')['成绩'].mean()     # 每班平均成绩
# 计网1    81.5
# 计网2    90.0
# 计网3    95.0

df.groupby('班级')['成绩'].agg(['mean', 'max', 'min', 'count'])
#          mean  max  min  count
# 计网1    81.5   85   78      2
# 计网2    90.0   90   90      1
# 计网3    95.0   95   95      1

# 多列多统计
df.groupby('班级').agg({
    '成绩': ['mean', 'max'],
    '年龄': ['mean', 'min']
})
```

### 3.8 处理缺失值

```python
# ============ 发现缺失值 ============
df.isnull()               # True/False矩阵，哪里是空
df.isnull().sum()         # 每列空值个数
df.isnull().sum().sum()   # 总空值数
df.notnull()              # 非空的位置

# ============ 删除缺失值 ============
df.dropna()                       # 删除任何含空值的行
df.dropna(axis=1)                 # 删除含空值的列
df.dropna(how='all')              # 只删全空的行
df.dropna(subset=['成绩'])        # 只看"成绩"列有无空值

# ============ 填充缺失值 ============
df.fillna(0)                      # 全部填0
df.fillna(df.mean())              # 每列填该列平均值
df['成绩'].fillna(df['成绩'].mean())  # 只填成绩列

# 前后值填充
df.fillna(method='ffill')         # 用前一个值填充（向前填）
df.fillna(method='bfill')         # 用后一个值填充（向后填）
```

### 3.9 合并与连接

```python
# ============ concat：简单拼接 ============
df1 = pd.DataFrame({'A':[1,2], 'B':[3,4]})
df2 = pd.DataFrame({'A':[5,6], 'B':[7,8]})

pd.concat([df1, df2])              # 竖向拼接
pd.concat([df1, df2], axis=1)      # 横向拼接

# ============ merge：类似SQL的JOIN ============
left = pd.DataFrame({'学号':[1,2,3], '姓名':['张三','李四','王五']})
right = pd.DataFrame({'学号':[1,2,4], '成绩':[85,90,78]})

pd.merge(left, right, on='学号')            # 内连接（只匹配两边都有的）
# 学号  姓名  成绩
# 1    张三  85
# 2    李四  90

pd.merge(left, right, on='学号', how='left')   # 左连接（保留左边所有行）
pd.merge(left, right, on='学号', how='right')  # 右连接
pd.merge(left, right, on='学号', how='outer')  # 全连接（保留所有行）
```

### 3.10 应用函数

```python
# ============ apply：对列/行应用函数 ============
df['成绩等级'] = df['成绩'].apply(
    lambda x: '优' if x>=90 else '良' if x>=80 else '中' if x>=70 else '差'
)

# ============ map：对Series逐元素映射 ============
df['班级代码'] = df['班级'].map({'计网1':1, '计网2':2, '计网3':3})

# ============ applymap：对整个DataFrame逐元素 ============
df.select_dtypes(include=['number']).applymap(lambda x: f'{x:.1f}')
```

### 3.11 保存数据

```python
df.to_csv('output.csv', index=False, encoding='utf-8-sig')   # CSV，不保存索引，中文编码
df.to_excel('output.xlsx', index=False, sheet_name='数据')    # Excel
df.to_json('output.json')                                     # JSON
df.to_html('output.html')                                     # HTML表格
```

---

## 第四章 OpenCV —— 图像基础操作

### 4.1 什么是OpenCV

OpenCV是最流行的计算机视觉库。它能读取图片、处理图片、检测人脸、识别物体。Python中用`cv2`模块调用。

**图像在计算机里的表示：** 一个三维数组（高×宽×3通道），每个像素是0~255的数值。

### 4.2 读取、显示、保存

```python
import cv2
import numpy as np

# ============ 读取图片 ============
img = cv2.imread('photo.jpg')              # 彩色读取（BGR格式）
img_gray = cv2.imread('photo.jpg', 0)      # 灰度读取（单通道）
img_color = cv2.imread('photo.jpg', 1)     # 彩色（默认，同上）

# imread返回None表示读取失败
if img is None:
    print('图片读取失败！请检查路径')

# ============ 显示图片 ============
cv2.imshow('原图', img)                     # 显示，窗口名+图片
cv2.imshow('灰度', img_gray)
cv2.waitKey(0)                              # 等待按键，0=无限等待
cv2.destroyAllWindows()                     # 关闭所有窗口

# waitKey参数：
cv2.waitKey(1000)   # 等待1000毫秒（1秒）后自动关闭

# ============ 保存图片 ============
cv2.imwrite('gray.jpg', img_gray)          # 保存灰度图
cv2.imwrite('copy.jpg', img)               # 保存彩色图
```

### 4.3 图片属性与像素操作

```python
img = cv2.imread('photo.jpg')

# ============ 属性 ============
print(img.shape)    # (480, 640, 3) → 高480, 宽640, 3通道(BGR)
print(img.dtype)    # uint8 → 每个像素0~255
print(img.size)     # 480*640*3 = 921600 → 总数据量

h, w, ch = img.shape
print(f'高:{h} 宽:{w} 通道:{ch}')

# ============ 访问单个像素 ============
px = img[100, 200]         # 第100行第200列的像素值
print(px)                  # [B, G, R] 如 [45, 120, 200]

img[100, 200] = [0, 0, 255]  # 把该像素改成红色（BGR格式）

# 只访问某个通道
blue = img[100, 200, 0]    # 蓝色通道值
green = img[100, 200, 1]   # 绿色通道值
red = img[100, 200, 2]     # 红色通道值

# ============ 获取整个通道 ============
b_channel = img[:, :, 0]   # 蓝色通道整张图
g_channel = img[:, :, 1]   # 绿色通道
r_channel = img[:, :, 2]   # 红色通道

# 分离和合并通道
b, g, r = cv2.split(img)            # 分离3个通道
img_merged = cv2.merge([b, g, r])   # 合并回来

# 只保留红色通道（其他通道置0）
red_only = cv2.merge([np.zeros_like(b), np.zeros_like(g), r])
cv2.imshow('红色通道', red_only)
```

### 4.4 颜色转换

```python
img = cv2.imread('photo.jpg')

# BGR → 灰度
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
print(gray.shape)    # (480, 640) → 只剩高和宽，没有通道了

# BGR → HSV（色相-饱和度-明度，常用于颜色检测）
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)

# BGR → RGB（matplotlib显示需要RGB）
rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

# ============ 用matplotlib显示OpenCV图片 ============
import matplotlib.pyplot as plt

plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))  # 先转RGB再显示！
plt.title('OpenCV图片在matplotlib中显示')
plt.axis('off')      # 不显示坐标轴
plt.show()

# ============ HSV颜色范围（常用于颜色检测）============
# 红色：H 0~10 或 170~180, S 100~255, V 100~255
# 绿色：H 35~85, S 100~255, V 100~255
# 蓝色：H 100~130, S 100~255, V 100~255

# 检测红色物体示例
lower_red = np.array([0, 100, 100])
upper_red = np.array([10, 255, 255])
mask = cv2.inRange(hsv, lower_red, upper_red)       # 红色区域为白色，其他黑色
red_area = cv2.bitwise_and(img, img, mask=mask)     # 只保留红色部分
cv2.imshow('红色区域', red_area)
```

### 4.5 缩放、裁剪、翻转

```python
img = cv2.imread('photo.jpg')

# ============ 缩放 ============
# 指定目标尺寸
small = cv2.resize(img, (320, 240))              # 宽320 高240

# 按比例缩放
half = cv2.resize(img, None, fx=0.5, fy=0.5)     # 宽高各缩一半
double = cv2.resize(img, None, fx=2.0, fy=2.0)   # 宽高各放大2倍

# 保持宽高比缩放（只指定一个维度）
h, w = img.shape[:2]
new_w = 400
new_h = int(h * new_w / w)
resized = cv2.resize(img, (new_w, new_h))

# ============ 裁剪 ============
# 用数组切片裁剪（注意：行=高度=y轴，列=宽度=x轴）
crop = img[100:300, 200:400]    # y从100到300，x从200到400
# 100:300 → 高度方向（行），裁出200像素高
# 200:400 → 宽度方向（列），裁出200像素宽

# 从中心裁剪正方形
h, w = img.shape[:2]
size = min(h, w)
start_y = (h - size) // 2
start_x = (w - size) // 2
square_crop = img[start_y:start_y+size, start_x:start_x+size]

# ============ 翻转 ============
flip_h = cv2.flip(img, 1)       # 水平翻转（左右镜像）→ 自拍镜像
flip_v = cv2.flip(img, 0)       # 垂直翻转（上下镜像）
flip_both = cv2.flip(img, -1)   # 同时翻转（旋转180度）

# ============ 旋转 ============
h, w = img.shape[:2]
center = (w//2, h//2)
M = cv2.getRotationMatrix2D(center, 45, 1.0)    # 中心点，角度45，缩放1.0
rotated = cv2.warpAffine(img, M, (w, h))         # 执行旋转

# 顺时针90度旋转
rot90 = cv2.rotate(img, cv2.ROTATE_90_CLOCKWISE)
# 逆时针90度
rot90_ccw = cv2.rotate(img, cv2.ROTATE_90_COUNTERCLOCKWISE)
```

### 4.6 画图标注

```python
img = cv2.imread('photo.jpg')

# ============ 画线 ============
cv2.line(img, (0, 0), (300, 300), (0, 255, 0), 3)
# 参数：图片，起点，终点，颜色(BGR)，线宽

# ============ 画矩形 ============
cv2.rectangle(img, (50, 50), (200, 200), (255, 0, 0), 2)
# 参数：图片，左上角，右下角，颜色，线宽
# 线宽=-1 → 填充矩形

cv2.rectangle(img, (50, 50), (200, 200), (255, 0, 0), -1)  # 填充

# ============ 画圆 ============
cv2.circle(img, (150, 150), 80, (0, 0, 255), 2)
# 参数：图片，圆心，半径，颜色，线宽
# -1 → 填充

# ============ 画椭圆 ============
cv2.ellipse(img, (150, 150), (100, 50), 0, 0, 360, (255,255,0), 2)
# 参数：图片，中心，(长轴,短轴)，旋转角度，起始角度，结束角度，颜色，线宽

# ============ 写文字 ============
cv2.putText(img, 'Hello OpenCV', (50, 50),
            cv2.FONT_HERSHEY_SIMPLEX, 1, (255,255,255), 2)
# 参数：图片，文字，位置，字体，大小，颜色，线宽

# 字体选项：
# FONT_HERSHEY_SIMPLEX   → 简单字体
# FONT_HERSHEY_PLAIN     → 小字体
# FONT_HERSHEY_COMPLEX   → 复杂字体

# ============ 画多边形 ============
pts = np.array([[10,10],[100,10],[100,100],[10,100]], np.int32)
pts = pts.reshape((-1, 1, 2))    # OpenCV需要的格式
cv2.polylines(img, [pts], True, (0,255,255), 2)  # True=闭合多边形

cv2.imshow('标注图', img)
cv2.waitKey(0)
```

### 4.7 滤波与降噪

```python
img = cv2.imread('photo.jpg')

# ============ 均值模糊 ============
blur = cv2.blur(img, (5, 5))             # 5×5均值模糊
# 核越大越模糊

# ============ 高斯模糊 ============
gauss = cv2.GaussianBlur(img, (5, 5), 0)  # 5×5高斯模糊，0=自动计算sigma
# 高斯模糊比均值模糊更自然，保留边缘更好

# ============ 中值模糊 ============
median = cv2.medianBlur(img, 5)          # 5×5中值模糊
# 中值模糊最适合去除椒盐噪声（黑白噪点）

# ============ 双边滤波 ============
bilateral = cv2.bilateralFilter(img, 9, 75, 75)
# 保边滤波：模糊但保留边缘，像美颜效果
# 参数：图片，邻域直径，颜色sigma，空间sigma
```

### 4.8 边缘检测

```python
img_gray = cv2.imread('photo.jpg', 0)

# ============ Canny边缘检测（最常用）============
edges = cv2.Canny(img_gray, 100, 200)
# 参数：灰度图，低阈值，高阈值
# 像素值>高阈值 → 边缘
# 像素值在低高之间且连接边缘 → 边缘
# 像素值<低阈值 → 不是边缘

# 调整阈值：值越小边缘越多，值越大边缘越少

# ============ Sobel边缘检测 ============
sobel_x = cv2.Sobel(img_gray, cv2.CV_64F, 1, 0, ksize=3)  # x方向
sobel_y = cv2.Sobel(img_gray, cv2.CV_64F, 0, 1, ksize=3)  # y方向
sobel = np.sqrt(sobel_x**2 + sobel_y**2)                    # 合起来

# ============ Laplacian ============
lap = cv2.Laplacian(img_gray, cv2.CV_64F)

cv2.imshow('Canny', edges)
cv2.waitKey(0)
```

### 4.9 形态学操作

```python
img_gray = cv2.imread('photo.jpg', 0)

# 二值化（先变成黑白图）
ret, binary = cv2.threshold(img_gray, 127, 255, cv2.THRESH_BINARY)
# 参数：灰度图，阈值，最大值，类型
# 像素>127 → 变255(白)，否则→0(黑)

kernel = np.ones((5,5), np.uint8)   # 5×5的核

# 腐蚀：缩小白色区域，去除小噪点
eroded = cv2.erode(binary, kernel, iterations=1)

# 膨胀：扩大白色区域，填补小空洞
dilated = cv2.dilate(binary, kernel, iterations=1)

# 开运算 = 腐蚀+膨胀：去噪点
opened = cv2.morphologyEx(binary, cv2.MORPH_OPEN, kernel)

# 闭运算 = 膨胀+腐蚀：填空洞
closed = cv2.morphologyEx(binary, cv2.MORPH_CLOSE, kernel)
```

### 4.10 视频操作

```python
# ============ 读取视频 ============
cap = cv2.VideoCapture('video.mp4')     # 视频文件
# cap = cv2.VideoCapture(0)             # 摄像头（0=默认摄像头）

while True:
    ret, frame = cap.read()             # 逐帧读取
    if not ret:
        break                           # 读完了
    
    cv2.imshow('视频', frame)
    if cv2.waitKey(25) & 0xFF == ord('q'):   # q退出
        break

cap.release()                           # 释放资源
cv2.destroyAllWindows()

# 视频属性
fps = cap.get(cv2.CAP_PROP_FPS)         # 帧率
width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))   # 宽
height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))  # 高
total_frames = int(cap.get(cv2.CAP_PROP_FRAME_COUNT))  # 总帧数

# ============ 保存视频 ============
fourcc = cv2.VideoWriter_fourcc(*'mp4v')   # 编码格式
out = cv2.VideoWriter('output.mp4', fourcc, fps, (width, height))

while True:
    ret, frame = cap.read()
    if not ret: break
    out.write(frame)                       # 写入帧

out.release()
```

---

## 第五章 Sklearn —— 机器学习入门

### 5.1 什么是机器学习

机器学习就是让计算机从数据中自动学出规律，而不是人手动写规则。

**三大类型：**
- **分类**：预测类别（如：判断邮件是否垃圾邮件）
- **回归**：预测数值（如：预测房价）
- **聚类**：自动分组（如：客户分群）

### 5.2 数据准备

```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, MinMaxScaler

# ============ 生成模拟数据 ============
# 分类数据：4个特征，3个类别，150个样本
from sklearn.datasets import make_classification
X, y = make_classification(n_samples=150, n_features=4, n_classes=3, n_informative=3, random_state=42)

# 回归数据：4个特征，100个样本
from sklearn.datasets import make_regression
X_reg, y_reg = make_regression(n_samples=100, n_features=4, random_state=42)

# 经典数据集：鸢尾花（分类）
from sklearn.datasets import load_iris
iris = load_iris()
X_iris = iris.data    # 150×4特征矩阵
y_iris = iris.target  # 150个标签（0,1,2三类）

# 经典数据集：波士顿房价（回归）
from sklearn.datasets import fetch_california_housing
housing = fetch_california_housing()
X_house = housing.data
y_house = housing.target

# ============ 拆分训练集和测试集 ============
X_train, X_test, y_train, y_test = train_test_split(
    X, y, 
    test_size=0.2,        # 测试集占20%
    random_state=42,      # 固定随机种子，保证每次拆分一样
    stratify=y            # 分类时用，保证各类别比例一致
)

print(f'训练集: {X_train.shape}, 测试集: {X_test.shape}')
# 训练集: (120, 4), 测试集: (30, 4)

# ============ 标准化 ============
# StandardScaler：均值0，标准差1（最常用）
scaler_std = StandardScaler()
X_train_std = scaler_std.fit_transform(X_train)    # 训练集：计算均值标准差并转换
X_test_std = scaler_std.transform(X_test)           # 测试集：用训练集的均值标准差转换

# MinMaxScaler：缩到0~1之间
scaler_mm = MinMaxScaler()
X_train_mm = scaler_mm.fit_transform(X_train)
X_test_mm = scaler_mm.transform(X_test)

# ⚠️ 重要：测试集只能transform，不能fit_transform！
# 原因：要用训练集的统计量来转换测试集，否则数据泄露
```

### 5.3 分类算法详解

```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.tree import DecisionTreeClassifier
from sklearn.svm import SVC
from sklearn.linear_model import LogisticRegression
from sklearn.naive_bayes import GaussianNB
from sklearn.ensemble import RandomForestClassifier

# ============ K近邻（KNN）============
# 思想：找最近的K个邻居，看他们多数是什么类别就判什么类别
knn = KNeighborsClassifier(n_neighbors=3)   # k=3：看3个邻居
knn.fit(X_train_std, y_train)              # 训练
y_pred_knn = knn.predict(X_test_std)        # 预测
acc_knn = knn.score(X_test_std, y_test)     # 准确率
print(f'KNN准确率: {acc_knn:.2%}')

# k的选择：一般3~7，可以用交叉验证找最优k
from sklearn.model_selection import cross_val_score
for k in range(1, 21):
    knn = KNeighborsClassifier(n_neighbors=k)
    scores = cross_val_score(knn, X_train_std, y_train, cv=5)
    print(f'k={k}, 平均准确率={scores.mean():.3f}')

# ============ 决策树 ============
# 思想：像流程图一样，每个节点做一个判断，最终分到叶子节点出结果
dt = DecisionTreeClassifier(
    max_depth=5,           # 最大深度5（防止过拟合）
    min_samples_split=5,   # 分裂至少需要5个样本
    random_state=42
)
dt.fit(X_train_std, y_train)
print(f'决策树准确率: {dt.score(X_test_std, y_test):.2%}')

# 查看树结构
from sklearn.tree import plot_tree
import matplotlib.pyplot as plt
plt.figure(figsize=(12, 8))
plot_tree(dt, feature_names=iris.feature_names, filled=True)
plt.show()

# 特征重要性（哪个特征对分类最有用）
print(dt.feature_importances_)    # 数组，值越大越重要

# ============ SVM（支持向量机）============
# 思想：找一条最优的分界线（超平面），让两类之间的间隔最大
svm = SVC(kernel='rbf', C=1.0, gamma='scale')  # rbf核最常用
svm.fit(X_train_std, y_train)
print(f'SVM准确率: {svm.score(X_test_std, y_test):.2%}')

# kernel选择：
# 'linear' → 线性核（数据线性可分时用）
# 'rbf'    → 径向基核（最常用，能处理非线性）
# 'poly'   → 多项式核

# ============ 逻辑回归 ============
# 思想：用sigmoid函数把线性回归的结果映射到0~1之间，当作概率
lr = LogisticRegression(max_iter=200, random_state=42)
lr.fit(X_train_std, y_train)
print(f'逻辑回归准确率: {lr.score(X_test_std, y_test):.2%}')

# 查看预测概率
y_proba = lr.predict_proba(X_test_std)    # 每个样本属于各类的概率
print(y_proba[:3])                         # 前3个样本的概率

# ============ 朴素贝叶斯 ============
# 思想：假设各特征独立，用贝叶斯公式计算概率
nb = GaussianNB()
nb.fit(X_train_std, y_train)
print(f'朴素贝叶斯准确率: {nb.score(X_test_std, y_test):.2%}')

# ============ 随机森林（集成方法，效果好）============
# 思想：建很多棵决策树，投票决定结果
rf = RandomForestClassifier(
    n_estimators=100,     # 100棵树
    max_depth=5,
    random_state=42
)
rf.fit(X_train_std, y_train)
print(f'随机森林准确率: {rf.score(X_test_std, y_test):.2%}')

# 随机森林通常准确率最高，是首选
```

### 5.4 回归算法详解

```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score, mean_absolute_error

# ============ 线性回归 ============
# 思想：找一条直线（或超平面）最好地拟合数据
reg = LinearRegression()
reg.fit(X_train_std, y_reg_train)

y_pred = reg.predict(X_test_std)

# 评估指标
print(f'R²: {r2_score(y_reg_test, y_pred):.4f}')       # R²越接近1越好
print(f'MSE: {mean_squared_error(y_reg_test, y_pred):.4f}')  # MSE越小越好
print(f'MAE: {mean_absolute_error(y_reg_test, y_pred):.4f}') # MAE越小越好

# 查看回归系数
print(f'系数: {reg.coef_}')       # 每个特征的权重
print(f'截距: {reg.intercept_}')  # 常数项

# ============ 可视化回归结果 ============
plt.scatter(y_reg_test, y_pred)
plt.plot([y_reg_test.min(), y_reg_test.max()], 
         [y_reg_test.min(), y_reg_test.max()], 'r--')
plt.xlabel('真实值')
plt.ylabel('预测值')
plt.title('回归预测 vs 真实值')
plt.show()
```

### 5.5 聚类算法

```python
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

# ============ KMeans聚类 ============
# 思想：把数据分成K组，每组围绕一个中心点
kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
kmeans.fit(X)

labels = kmeans.labels_              # 每个样本的类别标签
centers = kmeans.cluster_centers_    # 3个聚类中心的坐标

# 轮廓系数评估聚类质量（越接近1越好）
score = silhouette_score(X, labels)
print(f'轮廓系数: {score:.3f}')

# ============ 可视化 ============
plt.scatter(X[:,0], X[:,1], c=labels, cmap='viridis', s=30)
plt.scatter(centers[:,0], centers[:,1], c='red', marker='x', s=200, label='中心')
plt.legend()
plt.title('KMeans聚类结果')
plt.show()

# ============ 选择最优K值（肘部法则）============
scores = []
for k in range(2, 10):
    km = KMeans(n_clusters=k, random_state=42, n_init=10)
    km.fit(X)
    scores.append(silhouette_score(X, km.labels_))

plt.plot(range(2,10), scores, 'o-')
plt.xlabel('K值')
plt.ylabel('轮廓系数')
plt.title('肘部法则')
plt.show()
# 选择轮廓系数最高的K值
```

### 5.6 评估指标详解

```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    confusion_matrix, classification_report,
    roc_curve, auc
)

y_pred = knn.predict(X_test_std)

# ============ 分类评估 ============
# 准确率：预测正确的比例
print(f'准确率: {accuracy_score(y_test, y_pred):.2%}')

# 精确率：预测为正类中真正为正的比例（查准率）
print(f'精确率: {precision_score(y_test, y_pred, average="weighted"):.2%}')

# 召回率：真正为正类中被预测为正的比例（查全率）
print(f'召回率: {recall_score(y_test, y_pred, average="weighted"):.2%}')

# F1值：精确率和召回率的调和平均
print(f'F1: {f1_score(y_test, y_pred, average="weighted"):.2%}')

# 混淆矩阵
cm = confusion_matrix(y_test, y_pred)
print('混淆矩阵:')
print(cm)
# [[10  1  0]
#  [ 0  9  1]
#  [ 0  0  9]]

# 可视化混淆矩阵
import seaborn as sns
plt.figure(figsize=(6,5))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues')
plt.xlabel('预测')
plt.ylabel('真实')
plt.show()

# 分类报告（一次性输出所有指标）
print(classification_report(y_test, y_pred))

# ============ 交叉验证 ============
from sklearn.model_selection import cross_val_score

scores = cross_val_score(knn, X, y, cv=5)    # 5折交叉验证
print(f'每折准确率: {scores}')
print(f'平均准确率: {scores.mean():.3f} ± {scores.std():.3f}')
```

### 5.7 模型保存与加载

```python
import joblib

# 保存模型
joblib.dump(knn, 'knn_model.pkl')
joblib.dump(scaler_std, 'scaler.pkl')   # 也要保存标准化器！

# 加载模型
loaded_model = joblib.load('knn_model.pkl')
loaded_scaler = joblib.load('scaler.pkl')

# 使用加载的模型
X_new_std = loaded_scaler.transform(X_new)    # 先标准化
y_new_pred = loaded_model.predict(X_new_std)  # 再预测
```

### 5.8 完整流程示例

```python
# ======== 一个完整的机器学习流程 ========
import numpy as np
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix
import seaborn as sns
import matplotlib.pyplot as plt

# 1. 加载数据
data = load_iris()
X, y = data.data, data.target

# 2. 拆分
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42, stratify=y)

# 3. 标准化
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# 4. 选模型+训练
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# 5. 交叉验证
cv_scores = cross_val_score(model, X_train, y_train, cv=5)
print(f'交叉验证: {cv_scores.mean():.3f} ± {cv_scores.std():.3f}')

# 6. 预测+评估
y_pred = model.predict(X_test)
print(f'测试准确率: {model.score(X_test, y_test):.2%}')
print(classification_report(y_test, y_pred, target_names=data.target_names))

# 7. 混淆矩阵可视化
cm = confusion_matrix(y_test, y_pred)
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', 
            xticklabels=data.target_names, yticklabels=data.target_names)
plt.xlabel('预测')
plt.ylabel('真实')
plt.show()

# 8. 保存
import joblib
joblib.dump(model, 'iris_rf.pkl')
joblib.dump(scaler, 'iris_scaler.pkl')
```

---

## 第六章 PyTorch —— 深度学习入门

### 6.1 什么是深度学习

深度学习是机器学习的子集，用**多层神经网络**自动从数据中提取特征。PyTorch是最流行的深度学习框架之一，特点是：
- 动态计算图（写代码即定义网络，调试方便）
- 自动求导（autograd，不用手动算梯度）
- Python风格，写起来自然

### 6.2 张量基础

```python
import torch
import numpy as np

# ============ 创建张量 ============
# 从列表
a = torch.tensor([1, 2, 3])
print(a)                              # tensor([1, 2, 3])
print(a.dtype)                        # torch.int64

# 从列表（指定浮点型）
b = torch.tensor([1.0, 2.0, 3.0])
print(b.dtype)                        # torch.float32

# 指定类型
c = torch.tensor([1, 2, 3], dtype=torch.float32)
d = torch.tensor([1, 2, 3], dtype=torch.int32)

# ============ 快速创建 ============
zeros = torch.zeros(3, 4)             # 3×4全0
ones = torch.ones(2, 3)               # 2×3全1
eye = torch.eye(3)                    # 3×3单位矩阵
rand = torch.rand(3, 3)               # 3×3随机0~1
randn = torch.randn(3, 3)             # 3×3正态分布
randint = torch.randint(0, 10, (3,3)) # 3×3随机整数0~9
arange = torch.arange(0, 10, 2)       # [0, 2, 4, 6, 8]
linspace = torch.linspace(0, 1, 5)    # [0, 0.25, 0.5, 0.75, 1]
full = torch.full((2, 3), 7)          # 2×3全填7

# ============ 从NumPy创建 ============
arr = np.array([1, 2, 3])
t1 = torch.from_numpy(arr)            # numpy → tensor（共享内存）
t2 = torch.tensor(arr)                # numpy → tensor（复制数据）

# tensor → numpy
t = torch.tensor([1.0, 2.0, 3.0])
arr2 = t.numpy()                      # 转回numpy（共享内存）

# ============ 张量属性 ============
x = torch.randn(3, 4, 5)             # 三维张量
print(x.shape)                        # (3, 4, 5) → 形状
print(x.ndim)                         # 3 → 维度数
print(x.dtype)                        # torch.float32 → 数据类型
print(x.device)                       # cpu → 在哪个设备上

# ============ 形状操作 ============
x = torch.arange(12)                  # [0,1,...,11]

x.reshape(3, 4)                       # 3×4
x.reshape(2, 6)                       # 2×6
x.reshape(-1, 3)                      # 自动算行数
x.view(3, 4)                          # 和reshape类似，但要求内存连续

x.flatten()                           # 展平成一维

# 拼接
a = torch.tensor([[1,2],[3,4]])
b = torch.tensor([[5,6],[7,8]])

torch.cat([a, b], dim=0)              # 竖向拼接 → (4,2)
torch.cat([a, b], dim=1)              # 横向拼接 → (2,4)
torch.stack([a, b])                   # 新维度堆叠 → (2,2,2)
```

### 6.3 张量运算

```python
x = torch.tensor([1.0, 2.0, 3.0])
y = torch.tensor([4.0, 5.0, 6.0])

# ============ 逐元素运算 ============
x + y                  # [5, 7, 9]    → 加
x - y                  # [-3, -3, -3] → 减
x * y                  # [4, 10, 18]  → 逐元素乘
x / y                  # [0.25, 0.4, 0.5]
x ** 2                 # [1, 4, 9]    → 平方

torch.sqrt(x)          # 开方
torch.exp(x)           # 指数eˣ
torch.log(x)           # 对数ln(x)
torch.abs(x)           # 绝对值

# ============ 矩阵运算 ============
A = torch.tensor([[1.0, 2.0], [3.0, 4.0]])
B = torch.tensor([[5.0, 6.0], [7.0, 8.0]])

A @ B                  # 矩阵乘法
torch.matmul(A, B)     # 同上
torch.mm(A, B)         # 同上（只支持2D）

A.T                    # 转置

# ============ 统计 ============
torch.sum(x)           # 6.0    → 求和
torch.mean(x)          # 2.0    → 平均
torch.max(x)           # 3.0    → 最大值
torch.min(x)           # 1.0    → 最小值
torch.std(x)           # 标准差
torch.var(x)           # 方差

# 返回值和索引
max_val, max_idx = torch.max(x, dim=0)   # 最大值和位置

# 按维度统计
A = torch.randn(3, 4)
torch.sum(A, dim=0)     # 每列求和 → 形状(4,)
torch.sum(A, dim=1)     # 每行求和 → 形状(3,)
torch.mean(A, dim=0)    # 每列平均

# ============ 广播 ============
a = torch.randn(2, 3)
b = torch.randn(3)
a + b                   # b自动扩展成(2,3)
```

### 6.4 GPU加速

```python
# ============ 检查GPU ============
print(torch.cuda.is_available())        # True/False
print(torch.cuda.device_count())        # GPU数量
print(torch.cuda.get_device_name(0))    # GPU名称

# ============ 选择设备 ============
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
print(f'使用设备: {device}')

# ============ 张量移到GPU ============
x = torch.randn(3, 3)
x_gpu = x.to(device)                    # 移到GPU
# 或者
x_gpu = x.cuda()                        # 直接移到GPU（如果没有GPU会报错）

# GPU上的运算结果也在GPU
y_gpu = x_gpu * 2

# 回CPU
y_cpu = y_gpu.cpu()                     # 移回CPU

# ============ 模型移到GPU ============
model = MyNet()
model = model.to(device)                # 模型移到GPU

X_train = X_train.to(device)            # 数据也移到GPU
y_train = y_train.to(device)
```

### 6.5 自动求导（autograd）

```python
# ============ 基本用法 ============
x = torch.tensor(2.0, requires_grad=True)   # 要求追踪梯度

y = x ** 2                                  # y = x²
y.backward()                                # 反向传播，计算梯度
print(x.grad)                               # dy/dx = 2x = 4.0

# ============ 复合函数 ============
x = torch.tensor([1.0, 2.0, 3.0], requires_grad=True)
y = x ** 2 + 3 * x                         # y = x² + 3x
z = y.sum()                                 # z = Σy

z.backward()                                # 反向传播
print(x.grad)                               # dz/dx = 2x+3 → [5, 7, 9]

# ============ 多次求导要清零 ============
x = torch.tensor([2.0], requires_grad=True)

y = x ** 2
y.backward()
print(x.grad)                               # 4.0

# 再求一次梯度前必须清零！否则会累加
x.grad.zero_()

y = x ** 3
y.backward()
print(x.grad)                               # 12.0（不是4+12=16）

# ============ 不需要梯度时 ============
with torch.no_grad():                       # 临时关闭梯度追踪
    y = x * 2                               # 这里的运算不记录梯度
    
# 永久关闭
x.requires_grad_(False)                     # 不再追踪
```

### 6.6 构建神经网络详解

```python
import torch.nn as nn

# ============ nn.Linear：全连接层 ============
# nn.Linear(in_features, out_features)
# 输入4维 → 输出16维：nn.Linear(4, 16)
# 权重矩阵形状：(out_features, in_features) = (16, 4)
# 前向计算：output = input @ weight.T + bias

# ============ 常用激活函数 ============
nn.ReLU()                # max(0, x) → 最常用，简单高效
nn.Sigmoid()             # 1/(1+e⁻ˣ) → 输出0~1，用于二分类输出层
nn.Tanh()                # (eˣ-e⁻ˣ)/(eˣ+e⁻ˣ) → 输出-1~1
nn.LeakyReLU(0.01)       # max(0.01x, x) → ReLU变体，避免死神经元
nn.Softmax(dim=1)        # 多分类输出层，输出概率

# ============ Sequential方式（简单网络）============
model = nn.Sequential(
    nn.Linear(4, 16),       # 第1层：输入4→隐藏16
    nn.ReLU(),              # 激活
    nn.Linear(16, 8),       # 第2层：隐藏16→隐藏8
    nn.ReLU(),
    nn.Linear(8, 3)         # 输出层：隐藏8→输出3（3类分类）
)

# 查看模型结构
print(model)
# Sequential(
#   (0): Linear(in_features=4, out_features=16)
#   (1): ReLU()
#   (2): Linear(in_features=16, out_features=8)
#   (3): ReLU()
#   (4): Linear(in_features=8, out_features=3)
)

# 查看参数数量
total_params = sum(p.numel() for p in model.parameters())
print(f'总参数: {total_params}')

# ============ 自定义类方式（灵活网络）============
class MyNet(nn.Module):
    def __init__(self, input_size=4, hidden_size=16, output_size=3):
        super().__init__()
        self.fc1 = nn.Linear(input_size, hidden_size)
        self.fc2 = nn.Linear(hidden_size, 8)
        self.fc3 = nn.Linear(8, output_size)
        self.relu = nn.ReLU()
    
    def forward(self, x):
        x = self.relu(self.fc1(x))    # 第1层+激活
        x = self.relu(self.fc2(x))    # 第2层+激活
        x = self.fc3(x)               # 输出层（不加激活，CrossEntropyLoss自带Softmax）
        return x

model = MyNet(input_size=4, hidden_size=16, output_size=3)

# ============ Dropout（防止过拟合）============
class MyNetDropout(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(4, 16)
        self.dropout = nn.Dropout(0.5)    # 训练时随机丢弃50%神经元
        self.fc2 = nn.Linear(16, 3)
    
    def forward(self, x):
        x = torch.relu(self.fc1(x))
        x = self.dropout(x)              # 只在训练时生效，eval时自动关闭
        x = self.fc2(x)
        return x

# ============ BatchNorm（加速训练+稳定）============
class MyNetBN(nn.Module):
    def __init__(self):
        super().__init__()
        self.fc1 = nn.Linear(4, 16)
        self.bn1 = nn.BatchNorm1d(16)    # 对16维输出做批归一化
        self.fc2 = nn.Linear(16, 3)
    
    def forward(self, x):
        x = torch.relu(self.bn1(self.fc1(x)))
        x = self.fc2(x)
        return x
```

### 6.7 损失函数详解

```python
# ============ 分类损失 ============
# CrossEntropyLoss：多分类（内部自带Softmax，模型输出层不需要加Softmax）
loss_fn = nn.CrossEntropyLoss()

# BCELoss：二分类（模型输出层需要加Sigmoid，输出0~1）
loss_fn_bce = nn.BCELoss()

# BCEWithLogitsLoss：二分类（内部自带Sigmoid，模型不需要加）
loss_fn_bce_logits = nn.BCEWithLogitsLoss()   # 更稳定，推荐用这个

# ============ 回归损失 ============
# MSELoss：均方误差（最常用）
loss_fn_mse = nn.MSELoss()

# L1Loss：绝对值误差
loss_fn_l1 = nn.L1Loss()

# SmoothL1Loss：Huber损失（MSE和L1的结合）
loss_fn_smooth = nn.SmoothL1Loss()

# ============ 使用示例 ============
y_pred = model(X)             # 模型输出
loss = loss_fn(y_pred, y)     # 计算损失
print(f'Loss: {loss.item():.4f}')
```

### 6.8 优化器详解

```python
import torch.optim as optim

# ============ SGD（随机梯度下降）============
optimizer = optim.SGD(model.parameters(), lr=0.01)

# SGD+动量（更稳定，推荐）
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.9)

# ============ Adam（最常用，自适应学习率）============
optimizer = optim.Adam(model.parameters(), lr=0.001)

# ============ 其他优化器 ============
optim.RMSprop(model.parameters(), lr=0.001)    # RMSprop
optim.Adagrad(model.parameters(), lr=0.01)     # Adagrad

# ============ 学习率调度器 ============
scheduler = optim.lr_scheduler.StepLR(optimizer, step_size=30, gamma=0.1)
# 每30个epoch，学习率乘0.1

scheduler = optim.lr_scheduler.ReduceLROnPlateau(optimizer, patience=5)
# loss连续5个epoch不下降，学习率减半

# 使用：在训练循环中
for epoch in range(100):
    train(...)
    scheduler.step()     # 更新学习率
```

### 6.9 DataLoader（数据加载）

```python
from torch.utils.data import TensorDataset, DataLoader

# ============ 从numpy/tensor创建 ============
X = torch.randn(100, 4)
y = torch.randint(0, 3, (100,))

dataset = TensorDataset(X, y)

# DataLoader：批量加载+自动打乱
train_loader = DataLoader(
    dataset,
    batch_size=16,       # 每批16个样本
    shuffle=True,        # 训练集打乱顺序
    num_workers=0        # 数据加载线程数（Windows建议0）
)

# ============ 使用DataLoader训练 ============
for batch_X, batch_y in train_loader:
    print(f'批次大小: {batch_X.shape}')    # (16, 4)
    
    y_pred = model(batch_X)
    loss = loss_fn(y_pred, batch_y)
    
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

# ============ 分训练集和测试集 ============
from sklearn.model_selection import train_test_split

X_np = np.random.randn(200, 4)
y_np = np.random.randint(0, 3, 200)

X_train_np, X_test_np, y_train_np, y_test_np = train_test_split(X_np, y_np, test_size=0.2, random_state=42)

X_train = torch.tensor(X_train_np, dtype=torch.float32)
y_train = torch.tensor(y_train_np, dtype=torch.long)    # 分类标签用long
X_test = torch.tensor(X_test_np, dtype=torch.float32)
y_test = torch.tensor(y_test_np, dtype=torch.long)

train_dataset = TensorDataset(X_train, y_train)
test_dataset = TensorDataset(X_test, y_test)

train_loader = DataLoader(train_dataset, batch_size=16, shuffle=True)
test_loader = DataLoader(test_dataset, batch_size=16, shuffle=False)
```

### 6.10 完整训练流程

```python
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import TensorDataset, DataLoader

# ======== 1. 准备数据 ========
X_train = torch.randn(120, 4)
y_train = torch.randint(0, 3, (120,))
X_test = torch.randn(30, 4)
y_test = torch.randint(0, 3, (30,))

train_dataset = TensorDataset(X_train, y_train)
train_loader = DataLoader(train_dataset, batch_size=16, shuffle=True)

# ======== 2. 构建模型 ========
model = nn.Sequential(
    nn.Linear(4, 32),
    nn.ReLU(),
    nn.Linear(32, 16),
    nn.ReLU(),
    nn.Linear(16, 3)
)

# ======== 3. 损失函数+优化器 ========
loss_fn = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=0.001)

# ======== 4. 训练 ========
epochs = 50
for epoch in range(epochs):
    model.train()                    # 训练模式（启用Dropout/BatchNorm）
    total_loss = 0
    
    for batch_X, batch_y in train_loader:
        # 前向传播
        y_pred = model(batch_X)
        loss = loss_fn(y_pred, batch_y)
        
        # 反向传播
        optimizer.zero_grad()        # 清零梯度（必须！）
        loss.backward()              # 计算梯度
        optimizer.step()             # 更新参数
        
        total_loss += loss.item()
    
    avg_loss = total_loss / len(train_loader)
    
    # 每10个epoch输出一次
    if (epoch+1) % 10 == 0:
        # ======== 5. 评估 ========
        model.eval()                 # 评估模式（关闭Dropout/BatchNorm用全局统计）
        with torch.no_grad():        # 不计算梯度（省内存+加速）
            y_pred_test = model(X_test)
            y_class = torch.argmax(y_pred_test, dim=1)
            acc = (y_class == y_test).float().mean()
        
        print(f'Epoch {epoch+1}/{epochs}, Loss: {avg_loss:.4f}, 准确率: {acc:.2%}')

# ======== 6. 保存模型 ========
torch.save(model.state_dict(), 'my_model.pth')
print('模型已保存')

# ======== 7. 加载模型 ========
model.load_state_dict(torch.load('my_model.pth'))
model.eval()

# ======== 8. 新数据预测 ========
X_new = torch.randn(5, 4)            # 5个新样本
with torch.no_grad():
    y_new = model(X_new)
    y_new_class = torch.argmax(y_new, dim=1)
print(f'预测结果: {y_new_class}')
```

---

## 附录A：速查对照表

| 库 | 核心对象 | 一句话概括 | 安装命令 |
|---|---------|----------|---------|
| NumPy | ndarray | 数组计算，矩阵运算 | pip install numpy |
| Matplotlib | plt | 画图可视化 | pip install matplotlib |
| Pandas | DataFrame | 表格数据，像Excel | pip install pandas |
| OpenCV | cv2 | 图像读写和处理 | pip install opencv-python |
| Sklearn | Estimator | 机器学习一把梭 | pip install scikit-learn |
| PyTorch | tensor+Module | 深度学习，自动求导 | pip install torch |

## 附录B：常用导入写法

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import cv2
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score, classification_report
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import TensorDataset, DataLoader
```

## 附录C：常见错误速查

| 错误 | 原因 | 解决 |
|------|------|------|
| `imshow`图片颜色不对 | OpenCV是BGR，matplotlib要RGB | `cv2.cvtColor(img, cv2.COLOR_BGR2RGB)` |
| `matplotlib`中文显示方框 | 默认字体不支持中文 | `plt.rcParams['font.sans-serif']=['SimHei']` |
| sklearn测试集用了`fit_transform` | 数据泄露 | 测试集只`transform` |
| PyTorch标签类型报错 | 标签应该是`long` | `y = torch.tensor(y, dtype=torch.long)` |
| 梯度累加 | 没清零 | 每次循环前`optimizer.zero_grad()` |
| `model.eval()`忘写 | Dropout/BatchNorm训练和评估行为不同 | 评估前必须`model.eval()` |
| `DataLoader`Windows报错 | `num_workers>0` | Windows设`num_workers=0` |
