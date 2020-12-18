# pandas用法总结

## 合并(concat,merge)

### concat

**纵向合并,列取交集**

```python
pd.concat([df2500,df4000,df7800],axis=0,join='inner',sort=False)
```

### merge

```python
>>> x
      Admdate    Chgdate   Sampdate  item    name
0  2019-12-10 2020-03-13 2019-12-13     1   1.raw
1  2019-12-04 2020-01-21 2019-12-24    12  12.raw
2  2019-12-02 2020-03-13 2019-12-13     3   3.raw
3  2019-12-02 2020-03-13 2019-12-13     6   6.raw
4  2019-12-02 2020-03-13 2019-12-24    11  11.raw
..        ...        ...        ...   ...     ...
56 2018-08-07 2018-08-09 2018-08-08  O2-4       4
57 2018-07-15 2018-09-12 2018-08-13  O2-5       5
58 2018-08-07 2018-08-27 2018-08-17  O2-6       6
59 2018-08-05 2018-09-06 2018-08-18  O2-7       7
60 2018-07-19 2018-08-23 2018-08-20  O2-8       8
>>> df
  item bu
0    1  x
1    x  y
2    y  z
>>> df.merge(x)
  item bu    Admdate    Chgdate   Sampdate   name
0    1  x 2019-12-10 2020-03-13 2019-12-13  1.raw
>>> df.merge(x,how='left')
  item bu    Admdate    Chgdate   Sampdate   name
0    1  x 2019-12-10 2020-03-13 2019-12-13  1.raw
1    x  y        NaT        NaT        NaT    NaN
2    y  z        NaT        NaT        NaT    NaN
>>> df.merge(x,how='outer')
    item   bu    Admdate    Chgdate   Sampdate    name
0      1    x 2019-12-10 2020-03-13 2019-12-13   1.raw
1      x    y        NaT        NaT        NaT     NaN
2      y    z        NaT        NaT        NaT     NaN
3     12  NaN 2019-12-04 2020-01-21 2019-12-24  12.raw
4      3  NaN 2019-12-02 2020-03-13 2019-12-13   3.raw
..   ...  ...        ...        ...        ...     ...
58  O2-4  NaN 2018-08-07 2018-08-09 2018-08-08       4
59  O2-5  NaN 2018-07-15 2018-09-12 2018-08-13       5
60  O2-6  NaN 2018-08-07 2018-08-27 2018-08-17       6
61  O2-7  NaN 2018-08-05 2018-09-06 2018-08-18       7
62  O2-8  NaN 2018-07-19 2018-08-23 2018-08-20       8

# 如果colname不同,需要用到on
>>> pd.merge(kl1, sl1, how='left', left_on='Assembly',right_on='Strain')
```

## 排序(sort_values)

**根据列值排序,取反,选择前20**

```python
df.sort_values('similar',ascending=False).head(20)
```

## 查找空值行

```python
>>> df[df.isna().T.any()]['name']
0        GEN191310SZ_2
1      GEN191310SZ_162
2       GEN191310SZ_22
3       GEN191310SZ_23
4       GEN191310SZ_24
            ...       
123     GEN191310SZ_93
124     GEN191310SZ_96
125     GEN191310SZ_97
126     GEN191310SZ_98
127     GEN191310SZ_99
Name: name, Length: 128, dtype: object
```

## 查找索引(np.where)

**根据值查找索引**

```python
# 全局查找
dmso_mean = df['12'][:4].mean()/2 
x = np.where(dff < dmso_mean)
for k,j in zip(x[0],x[1]):
    print('位于第{}行,第{}列'.format(k+1,j+1))
```

## 筛选数据(drop/loc)

**删除行/列**

```python
# 根据column或者index
df = df.drop('name',axis =1) #删除列 
df = df.drop([0,1]) # 删除行
# 根据值
df[~df['ST'].isin(['-'])]
```

```python
# 筛选,samples为列表
df.loc[df['item'].isin(samples)]
# 筛选单个数据
db.loc[(db['db'] == db) & (db['Row'] == row )]
# 匹配(包含)筛选
df['File'].loc[df['scheme'].str.contains('ab|kp')]
df['column3'][df.isnull().T.any()
```




## 读取文件(xlrd,parse_dates)

```python
# xls文件不能直接pandas读,需用xlrd,加上编码.
wb = xlrd.open_workbook(i,encoding_override="gb2312")
# 选用第二行作为header,跳过第一行.
df = pd.read_excel(wb,header=1)
# 日期读取
df = pd.read_csv('casedatabase.csv',parse_dates=['Admdate','Chgdate','Sampdate'])
# 获取年月日
date = df.iloc[1,3]
date.year,date.month,date.day
```

## 输出文件(ExcelWriter)

```python
# 输出多个sheet
>>> writer = pd.ExcelWriter('output.xlsx')
>>> df1.to_excel(writer,'Sheet1')
>>> df2.to_excel(writer,'Sheet2')
>>> writer.save()
```

## 数据合并(groupby)

```python
df_merge = df.groupby(by='著作').apply(lambda x: '、'.join(x['人物']))
# 合并为列为数字
df_merge = df.groupby(by='id').apply(lambda x: ','.join(x['counts'].apply(str)))

```

## 替换(apply/replace)

```python
# 将'Assembly'列-替换为空
kl['Assembly'] = kl['Assembly'].apply(lambda  x: x.replace('-',''))
```

