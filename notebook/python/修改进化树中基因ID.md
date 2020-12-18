```python
text = open(tree_file).read()

# 第一种
names = re.findall(r'[a-zA-Z0-9_.]+:',text)
for name in names:
    name_re = name.split(".")[0]+":"
    text = text.replace(name,name_re)

# 第二种解法,更精炼
re.sub(r'([a-zA-Z0-9_]+)\.[a-zA-Z0-9_.]+:',r'\1:',text)
```

