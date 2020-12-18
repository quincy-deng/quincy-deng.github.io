# 院内传播(nosott)分析

## 手动运行

### 文件准备

```bash
# 解压SCOTTI和beast软件包到/home/dengqiuyang/biosoft目录
tar zxvf SCOTTI_beast.tgz
# 软链接要用到的几个文件
find $(pwd) -name SCOTTI_generate_xml.py|xargs ln -s
find $(pwd) -name *alternative.py|xargs ln -s
find $(pwd) -name SC*jar|xargs ln -s
find $(pwd) -name jbl*jar|xargs ln -s
find $(pwd) -name gua*jar|xargs ln -s
find $(pwd) -name beast*jar|xargs ln -s
find $(pwd) -name java|xargs ln -s

# 安装parsnp和harvesttools
conda create -n nosott -c bioconda parsnp harvesttools

# 安装graphviz
sudo yun install graphviz
```

### 运行

```bash
# 生成核心基因组
parsnp -c -r NMDC60013901-01.fasta -d fasta -o parsnpResult
# -c 表示留下所以样本

# 提取fasta
harvesttools -x parsnpResult/parsnp.xmfa -M parsnp.fasta

# 生成xml文件
path="/home/dengqiuyang/biosoft"
python ${path}/SCOTTI_generate_xml.py -f parsnp.fasta -o scotti -d date/dates.csv -ho date/hosts.csv -ht date/hosttime.csv -ov -m 5 -n 100000

# 生成tree
path="/home/dengqiuyang/biosoft"
${path}/java -cp  ${path}/SCOTTI.v2.0.1.jar:${path}/jblas-1.2.3.jar:${path}/guava-15.0.jar:${path}/beast.jar beast.app.beastapp.BeastMain -overwrite scotti.xml

#生成图片
path="/home/dengqiuyang/biosoft"
python ${path}/Make_transmission_tree_alternative.py --input scotti.trees --outputF disease --minValue 0.2
```

#### 修改输出

```python
# 修改Make_transmission_tree_alternative.py
## 由于样本过多,显示所有样本不利于观测,在生成graph文件循环中加入判断,如果样本没有传播关系,就不输出.
if not G[i]:
    continue
```

