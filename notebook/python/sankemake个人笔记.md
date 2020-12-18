## 常见报错

### Target rules may not contain wildcards

原因:

没有写rule all;

samples必须指定,不能直接写通配符

## 遇到的问题

### 文件名有多种后缀

> 如 .R1.fastq.gz  _1.fastq.gz  _1.fq.gz ...

参考别人的方案,把通配符写在shell命令里

### config yaml文件

**KEY不可以是数字**