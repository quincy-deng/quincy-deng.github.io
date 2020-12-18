### 去重排序

它的作用很简单，就是在输出行中去除重复行。

```bash
$ sort seq.txt
apple
orange
pear
pear
$ sort -u seq.txt
apple
orange
pear
```

### 反向排序

sort默认的排序方式是升序，如果想改成降序，就加个-r就搞定了。

### 数字大小

```bash
$ sort number.txt
10
11
2
$ sort -n number.txt
2
10
11
```

### 分割符 | 排序项

```bash
$ sort -n -k 2 -t : facebook.txt
apple:10:2.5
orange:20:3.4
banana:30:5.5
pear:90:2.3
```

