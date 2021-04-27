# 10m-2-pandas-visualization
10分钟上手pandas第二部分，数据可视化

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
