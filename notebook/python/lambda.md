**根据p_change列的与0的大小关系来生成新的一列‘涨跌’。 大于0时为涨， 等于0时为平，小于0时为跌**

```python
df['涨跌'] = df['p_change'].apply(lambda x: (x<0 and '跌') or (x==0 and '平') or (x>0 and '涨'))
```

