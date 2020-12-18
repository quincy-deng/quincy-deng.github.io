sed常用命令总结
---------------------------------------------------------------------------------------------------------

> 2020-01-21 

### 打印

```bash
$ sed -n '1,3p' fruit.txt
  # np连用,打印1到3行
$ sed -n '5,/^test/p' file
  # 打印从第5行开始到第一个包含以test开始的行之间的所有行
$ sed -n '1~2p' test.txt
  # 奇数行 
$ sed -n '2~2p' test.txt
  # 偶数行 
```

### 替换

```bash
$ sed 's/book/books/' file
  # 替换并打印
$ sed -i 's/book/books/' file
  # 直接编辑文件
$ echo sksksksksksk | sed 's/sk/SK/2g' 
skSKSKSKSKSK
  # 从第2处匹配开始替换
$ sed '/test/,/west/s/$/aaa bbb/' file
  # 对于模板test和west之间的行，每行的末尾用字符串aaa bbb替换
$  sed -e '1,5d' -e 's/test/check/' file
  # 第一条命令删除1至5行，第二条命令用check替换test
$ for i in *fasta;do sed -i "s/${i%.fasta}_//" $i;done
  # 使用变量加双引号
$ sed -i 's/genomic_.*strand/genomic/' hvct.txt
  # 正则替换
```

### 删除

```bash
$ sed '/^$/d' file
  # 删除空白行
$ sed '2,$d' file
  # 删除第二行到最后一行
$ sed '$d' file
  # 删除最后一行
$ sed '/^test/d' file
  # 删除以test开头的行
```

### 插入（行上,行下）

```bash
$ sed '/^test/i\this is a test line' file
  # i\命令 将 this is a test line 追加到以test开头的行前面
$ $ sed '/^test/a\this is a test line' file
  # a\命令 将 this is a test line 追加到以test开头的行后面
$ sed -i '5i\this is a test line' test.conf
  # 在第5行前插入this is a test line
```

### 进阶

```bash
$ echo this is a test line 22| sed 's/\w\+/[&]/g'
[this] [is] [a] [test] [line] [22]
  # 已匹配字符串标记&; \w\+ 匹配每一个单词，使用 [&] 替换它，& 对应于之前所匹配到的单词
$  echo this is digit 7 in a number | sed 's/digit \([0-9]\)/\1/' 
this is 7 in a number
  # 子串匹配标记\1; digit 7，被替换成了 7。
$ echo aaa BBB CCC| sed 's/\([a-z]\+\) \([A-Z]\+\)/\2 /'
BBB  CCC
  # 子串匹配标记\2
```

精简自[Linux之sed命令详解](https://www.linuxprobe.com/linux-sed-command.html)