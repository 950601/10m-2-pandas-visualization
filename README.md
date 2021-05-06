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




参考网址：
https://pandas.pydata.org/docs/user_guide/10min.html#getting-data-in-out
https://pandas.pydata.org/pandas-docs/stable/user_guide/visualization.html
