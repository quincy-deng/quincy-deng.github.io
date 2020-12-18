awk用法总结
-------------------------------------------------------------------------------------

### 多个分隔符

```bash
# some.log 文件内容如下
Grape(100g)1980
raisins(500g)1990
plum(240g)1997
apricot(180g)2005
nectarine(200g)2008
$ awk -F '[()]' '{print $1, $2, $3}' some.log
Grape 100g 1980
raisins 500g 1990
plum 240g 1997
apricot 180g 2005
nectarine 200g 2008
```

### 内置变量的使用

```bash
# fruit.log 文件内容如下
peach 100 Mar 1997 China
Lemon 150 Jan 1986 America
Pear 240 Mar 1990 Janpan
avocado 120 Feb 2008 china

$ awk '{$2 = "***"; print $0}' fruit.txt
peach *** Mar 1997 China
Lemon *** Jan 1986 America
Pear *** Mar 1990 Janpan
avocado *** Feb 2008 china

$ awk '{print $(NF - 1)}' fruit.txt
1997
1986
1990
2008

# 打印文件名
$ awk '{print FILENAME "\t" $0}' fruit.txt
fruit.txt  peach 100 Mar 1997 China
fruit.txt  Lemon 150 Jan 1986 America
fruit.txt  Pear 240 Mar 1990 Janpan
fruit.txt  avocado 120 Feb 2008 china
```

### 在 awk 中使用变量

```bash
awk '{a = 12; b = 24; print a + b}' fruit.txt
36
36
36
36
```

### 在 awk 中使用条件判断

```bash
# company.txt 内容如下
yahoo 100 4500
google 150 7500
apple 180 8000
twitter 120 5000

$ awk '$3 < 5500' company.txt
$ awk '{if ($3 < 5500) print $0}' company.txt
  # 同样的效果
yahoo 100 4500
twitter 120 5000
```

### 在 awk 中使用正则表达式

```bash
# poetry.txt 内容如下
This above all: to thine self be trueThere is nothing either good or bad, but thinking makes it so
There’s a special providence in the fall of a sparrow
No matter how dark long, may eventually in the day arrival

$ awk '/There/{print $0}' poetry.txt
There is nothing either good or bad, but thinking makes it so
There’s a special providence in the fall of a sparrow

$ awk '/^Th[ie]/{print $0}' poetry.txt
  # [a-zA-Z] 表示匹配小写的 a 到 z 之间的单个字符，或者大写的 A 到 Z 之间的单个字符
  # [^a-z] 符号 `^` 在方括号里面表示取反，也就是非的意思，表示匹配任何非 a 到 z 之间的单个字符
This above all: to thine self be true
There is nothing either good or bad, but thinking makes it so
There’s a special providence in the fall of a sparrow

$ awk '/go{2}d/{print $0}' poetry.txt
There is nothing either good or bad, but thinking makes it so

$ awk '/ba?d/{print $0}' poetry.txt
There is nothing either good or bad, but thinking makes it so

# 圆括号的用法
$ awk '/th(in){1}king/{print $0}' poetry.txt
There is nothing either good or bad, but thinking makes it so
```

精简自[awk工具的使用方法](https://mp.weixin.qq.com/s?__biz=MzA4NzQzMzU4Mg==&mid=2652919679&idx=1&sn=ba290e6b22af8aaa2390cfe13c2cb195&chksm=8bed7f7ebc9af668ad14e171ec84d623c728bdfa05456f38e71d1ebf7fdb9e20ddb5cc0c72f3&mpshare=1&scene=24&srcid=0104M7DW95ndDezdyu5uMMSh&key=6234e09828e71f222e5626d30dc3c6c51adc0b1cbe06e6a27620711dd89cf5a9414e89a181b9cf282483cc1ea6bd731cdb59367d7e708717448f589ba929d8d846ee3a7c132d5e2ff2a6fac530c4e47c&ascene=14&uin=MjU0MTIyNDI2Mg%3D%3D&devicetype=Windows+10&version=62060833&lang=zh_CN&pass_ticket=cAOfFxO%2BcYw3imI8dJlDR81cg%2FNv%2BiKmGScVVN9m7Nv7Ek8PfalMMdRupPdTdgAF)