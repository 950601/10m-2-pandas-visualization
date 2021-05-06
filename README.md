# 10-minutes-to-pandas

## 首先引入包

```
import numpy as np
import pandas as pd
```

### 创建数据对象
> 数据对象一般包括Series,DateFrame，详细介绍：https://pandas.pydata.org/docs/user_guide/dsintro.html#dataframe

- **创建一个series对象，使用pandas默认索引**
> s = pd.Series([1, 3, 5, np.nan, 6, 8])


- **创建一个DateFrame对象，指定日期行头与大写字母列头，使用随意数据**
```
dates = pd.date_range("20130101", periods=6)
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list("ABCD"))
```

- **创建一个DateFrame对象，使用不同的类型**
```
df2 = pd.DataFrame(
       {
           "A": 1.0,
           "B": pd.Timestamp("20130102"),
           "C": pd.Series(1, index=list(range(4)), dtype="float32"),
           "D": np.array([3] * 4, dtype="int32"),
           "E": pd.Categorical(["test", "train", "test", "train"]),
           "F": "foo",
        }
    )
```


### 查阅数据 

- **查看前后数据**
```
df.head()
df.tail(3)  # default value is 5
```

- **查看列头，行头**

```
df.index
df.columns
```


- **DateFrame转NumPy Array**
> 由于DateFrame拥有多种类型，而NumPy只允许全部为一种类型，所以df2不推荐使用此方法
```
df.to_numpy()
df2.to_numpy()
```

- **快速统计**

```
df.describe()
```

- **行列调转（Transposing ）**

```
df.T
```
- **排序**
> axis = 1 代表以列排序，0 代表以行排序
> by = clounm 指定具体排序列
```
df.sort_index(axis=1, ascending=False)
df.sort_values(by="B")
```




### 获取数据


- **根据名称和位置获取**

> 获取A列数据，返回DataFrame类型，索引（index）为日期
```
df["A"] / df.A
```
> 获取0-3行数据
```
df[0:3]
```
> 获取20130102-20130104行数据
```
df["20130102":"20130104"]
```
> 获取第3行数据
```
df.iloc[3]
```
> 获取第3-5行，0-2列数据
```
df.iloc[3:5, 0:2]
```
> 获取第1,2,4行，0-2列数据
```
df.iloc[[1, 2, 4], [0, 2]]
```
> 获取全部行，全部列
```
df.iloc[:, 1:3]
```
> 获取单个数据，高精度
```
df.iloc[1, 1]
```



- **多形式获取数据**
> 获取index = dates[0]的数据，返回DataFrame类型，索引为列头（A,B,C,D）
```
df.loc[dates[0]]
```

> 获取所有行，以及A,B列
```
df.loc[:, ["A", "B"]]
```

> 获取指定范围内的行与列
```
df.loc["20130102":"20130104", ["A", "B"]]
```

- **根据条件获取数据**
> 获取A列大于0的数据
```
df[df["A"] > 0]
```

> 获取DateFrame大于0的数据，小于0返回none
```
df[df > 0]
```

> 根据是否存在指定value获取数据
```
df2[df2["F"].isin(["foo"])]
```


- **设置获取数据**
> 在已有数据上加一列，根据index匹配行
```
s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range("20130102", periods=6))
df["F"] = s1
```

> 在指定位置设置value
```
df.at[dates[0], "A"] = 0
df.iat[0, 1] = 0
df.loc[:, "D"] = np.array([5] * len(df))
```

> 根据条件设置value
```
df2 = df.copy()
df2[df2 > 0] = -df2
```


### 数据过滤

- **增删改查**

> 重构DateFrame副本，reindex返回一个新的copy
```
df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ["E"])
df1.loc[dates[0] : dates[1], "E"] = 1
```

> 删除空行
```
df1.dropna(how="any")
```

> 填充空行
```
df1.fillna(value=5)
```

> 查看空行。由于副本不具备方法，所以使用原始DareFrame对象
```
pd.isna(df1)
```

### 数据操作

- **数据计算**

> 获取最小值，0代表列，1代表行
```
df.mean()
df.mean(1)
```

> 加减乘除
```
df.add(1)
df.div(1)
df.sub(1)
df.mul(1)
```


- **函数计算**

> 获取累加值，0代表列，1代表行，累加定义：https://www.jianshu.com/p/23e7e9251abb
```
df.apply(np.cumsum，0)
```

> 获取每列的最大值-最小值的结果
```
df.apply(lambda x: x.max() - x.min())
```


- **特殊计算**

> 获取出现次数
```
s = pd.Series(np.random.randint(0, 7, size=10))
s.value_counts()
```

> 字符排序
```
s = pd.Series(["A", "B", "C", "Aaba", "Baca", np.nan, "CABA", "dog", "cat"])
s.str.lower()
```

### 数据合并

- **连接数据**

> 连接DataFrame片段，注意：DataFrame增加行速度快，但是增加Row需要复制数组，效率低
> 官方建议：https://pandas.pydata.org/docs/user_guide/merging.html#merging-concatenation
```
df = pd.DataFrame(np.random.randn(10, 4))
pieces = [df[:3], df[3:7], df[7:]]
pd.concat(pieces)
```

> 根据key合并DataFrame，k-v不重复的情况
```
left = pd.DataFrame({"key": ["foo", "foo"], "lval": [1, 2]})
right = pd.DataFrame({"key": ["foo", "foo"], "rval": [4, 5]})
pd.merge(left, right, on="key")
```

> 根据key合并DataFrame，k-v重复的情况
```
left = pd.DataFrame({"key": ["foo", "bar"], "lval": [1, 2]})
right = pd.DataFrame({"key": ["foo", "bar"], "rval": [4, 5]})
pd.merge(left, right, on="key")
```



### 分组

- **分组汇总**

> 根据相应列进行分组汇总

```
df = pd.DataFrame(
        {
            "A": ["foo", "bar", "foo", "bar", "foo", "bar", "foo", "foo"],
            "B": ["one", "one", "two", "three", "two", "two", "one", "three"],
            "C": np.random.randn(8),
            "D": np.random.randn(8),
        }
    )

df.groupby("A").sum() 
df.groupby(["A", "B"]).sum()
```

### 时间操作

- **时间取样**

> 按分秒取样

```
idx = pd.date_range('1/1/2000', periods=4, freq='T')
df = pd.DataFrame(data=4 * [range(2)],
                  index=idx,
                  columns=['a', 'b'])
df.iloc[2, 0] = 5
df.resample('3T').sum()
df.resample('30S').sum()
```

> 分组取样

```
df.groupby('a').resample('30S').sum()
```


> 时区操作

```
rng = pd.date_range("3/6/2012 00:00", periods=5, freq="D")
ts = pd.Series(np.random.randn(len(rng)), rng)
ts_utc = ts.tz_localize("UTC")
ts_utc.tz_convert("US/Eastern")
```

> 时间格式转换

```
rng = pd.date_range("1/1/2012", periods=5, freq="M")
ts = pd.Series(np.random.randn(len(rng)), index=rng)
ps = ts.to_period()
ps.to_timestamp()
```

> 使用函数转换时间格式

```
prng = pd.period_range("1990Q1", "2000Q4", freq="Q-NOV")
ts = pd.Series(np.random.randn(len(prng)), prng)
ts.index = (prng.asfreq("M", "e") + 1).asfreq("H", "s") + 9
ts.head()
```


### 字典类型

- **字典操作**

> DateFrame字段转Category类型

```
df = pd.DataFrame(
      {"id": [1, 2, 3, 4, 5, 6], "raw_grade": ["a", "b", "b", "a", "a", "e"]}
)
df["grade"] = df["raw_grade"].astype("category")
df["grade"]

```

> Category重命名

```
df["grade"].cat.categories = ["very good", "good", "very bad"]
```

> 按照类型排序，类型顺序是category定义的类型，不是字符自然排序

```
df.sort_values(by="grade")
```
> 类型分组统计

```
df.groupby("grade").size()
```



### Excel操作

- **导入导出**

```
pd.read_excel("foo.xlsx", "Sheet1", index_col=None, na_values=["NA"])
df.to_excel("foo.xlsx", sheet_name="Sheet1")
```



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
