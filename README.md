
### 数据可视化  
 
- **导入plot包**  
> 导入包     
``` 
import matplotlib.pyplot as plt    
plt.close("all") 
```   
 
- **画图**  
> 折线图 
``` 
ts = pd.Series(np.random.randn(1000), index=pd.date_range("1/1/2000", periods=1000))
ts = ts.cumsum()
ts.plot();
```

> 多曲线（带label）折线图
```
df = pd.DataFrame(np.random.randn(1000, 4), index=ts.index, columns=list("ABCD"))
df = df.cumsum()
plt.figure();
df.plot();
```
 

> 柱状图
```
df.iloc[1].plot.bar() 
plt.show()
```  

> 列聚合柱状图
``` 
df2 = pd.DataFrame(np.random.rand(6, 4), columns=["a", "b", "c", "d"])
df2.plot.bar()
plt.show()
```

> 列聚合柱状图（叠起聚合柱）
```
df2.plot.bar(stacked=True)
```

> 列聚合柱状图（水平视图）
```
df2.plot.barh(stacked=True)
```

> 直方图（一般表示质量高低） alpha表示透明度，y轴为出现频次，x轴为值
```
df4 = pd.DataFrame({
        "a": np.random.randn(1000) + 1,
        "b": np.random.randn(1000),
        "c": np.random.randn(1000) - 1,},columns = ["a", "b", "c"],)
       print(df4)
df4.plot.hist(alpha=0)
plt.show()
```       

> bins表示柱子出现的个数，平均X轴
```
df4.plot.hist(bins=20)
```

> 单列直方图
```
df4["a"].plot.hist(orientation="horizontal", cumulative=True)
```

> 通过by指定直方图数据，figsize代表宽高像素
```
data = pd.Series(np.random.randn(1000))
data.hist(by=np.random.randint(0, 4, 1000), figsize=(6, 4))
```

> 箱型图（https://baike.baidu.com/item/%E7%AE%B1%E5%BD%A2%E5%9B%BE/10671164?fr=aladdin）
```
df = pd.DataFrame(np.random.rand(10,5),columns=['A','B','C','D','E'])
f = df.boxplot(sym = 'o',            #异常点形状
               vert = True,          # 是否垂直
               whis=1.5,             # IQR
               patch_artist = True,  # 上下四分位框是否填充
               meanline = False,showmeans = True,  # 是否有均值线及其形状
               showbox = True,   # 是否显示箱线
               showfliers = True,  #是否显示异常值
               notch = False,    # 中间箱体是否缺口
               return_type='dict')  # 返回类型为字典
plt.title('箱线图',fontproperties=myfont)
plt.show()
```

> 箱型图分组绘图 by
```
df = pd.DataFrame(np.random.rand(10, 2), columns=["Col1", "Col2"])
df["X"] = pd.Series(["A", "A", "A", "A", "A", "B", "B", "B", "B", "B"])
plt.figure()
bp = df.boxplot(by="X")
plt.show()
```

> 箱型图多列分组绘图
```
df = pd.DataFrame(np.random.rand(10, 3), columns=["Col1", "Col2", "Col3"])
df["X"] = pd.Series(["A", "A", "A", "A", "A", "B", "B", "B", "B", "B"])
df["Y"] = pd.Series(["A", "B", "A", "B", "A", "B", "A", "B", "A", "B"])
plt.figure();
bp = df.boxplot(column=["Col1", "Col2"], by=["X", "Y"])
plt.show()
```

> 箱型图多列分组绘图
```
np.random.seed(1234)
df_box = pd.DataFrame(np.random.randn(50, 2))
df_box["g"] = np.random.choice(["A", "B"], size=50)
df_box.loc[df_box["g"] == "B", 1] += 3
bp = df_box.boxplot(by="g")
```

> 面积图
> 面积图数据只能全部为正数，或者全部为负数，如果为空，会被默认赋值为0，可以用dataframe.dropna() or dataframe.fillna()方法，解决空值
> stacked=true 堆叠面积图
```
df = pd.DataFrame(np.random.rand(10, 4), columns=["a", "b", "c", "d"])
df.plot.area((stacked=False))
```

> 散点图

```
df = pd.DataFrame(np.random.rand(50, 4), columns=["a", "b", "c", "d"])
df.plot.scatter(x="a", y="b")
```

> 分组散点图

```
ax = df.plot.scatter(x="a", y="b", color="DarkBlue", label="Group 1")
df.plot.scatter(x="c", y="d", color="DarkGreen", label="Group 2", ax=ax)
```

> 分组散点图 c=color s=size

```
df.plot.scatter(x="a", y="b", c="c", s=50)
```

> 通过列来指定 散点大小

```
df.plot.scatter(x="a", y="b", s=df["c"] * 200)
```


> 六边柱状图
> 六边柱状图 用于散点图点分布太多繁多的情况下使用

> 创建六边柱状图
> gridsize参数用于控制x轴的数据范围大小，默认值是100
```
df = pd.DataFrame(np.random.randn(1000, 2), columns=["a", "b"])
df["b"] = df["b"] + np.arange(1000)
df.plot.hexbin(x="a", y="b", gridsize=25)
```

> 用z列作为值列，a,b列作为定位列
```
df = pd.DataFrame(np.random.randn(1000, 2), columns=["a", "b"])
df["b"] = df["b"] = df["b"] + np.arange(1000)
df["z"] = np.random.uniform(0, 3, 1000)
df.plot.hexbin(x="a", y="b", C="z", reduce_C_function=np.max, gridsize=25)
```


> 饼图
> 在饼图中，空值将默认填充为0，如果值有负数，那么会抛出ValueError 

> 创建饼图
```
series = pd.Series(3 * np.random.rand(4), index=["a", "b", "c", "d"], name="series")
series.plot.pie(figsize=(6, 6))

```

> 设置坐标轴精度的范围
> legend 图例 subplots 子图（根据列来显示）
```
df = pd.DataFrame(3 * np.random.rand(4, 2), index=["a", "b", "c", "d"], columns=["x", "y"])
df.plot.pie(subplots=True, figsize=(8, 4))
```


> 设置扇形区域的字体以及显示
```
series.plot.pie(
     labels=["AA", "BB", "CC", "DD"],
     colors=["r", "g", "b", "c"],
     autopct="%.2f", # 格式化扇形的占比数字，输出两位小数
     fontsize=20, # 扇形内字体大小
     figsize=(6, 6),
   );
```
 

> 当数值占比不足100%时，饼图会画为半圆
```
series = pd.Series([0.1] * 4, index=["a", "b", "c", "d"], name="series2")
series.plot.pie(figsize=(6, 6))
```

> 散点矩阵图
> diagonal 斜线区域可选参数 => kde:折线 hist:直方图
```
from pandas.plotting import scatter_matrix
df = pd.DataFrame(np.random.randn(1000, 4), columns=["a", "b", "c", "d"])
scatter_matrix(df, alpha=0.2, figsize=(6, 6), diagonal="kde")
```

> 密度图
```
ser = pd.Series(np.random.randn(1000))
ser.plot.kde();
```

> 使用第三方画图
```
Series([1, 2, 3]).plot(backend="backend.module") 
```
 
 

 
参考网址：
https://pandas.pydata.org/docs/user_guide/10min.html#getting-data-in-out
https://pandas.pydata.org/pandas-docs/stable/user_guide/visualization.html
