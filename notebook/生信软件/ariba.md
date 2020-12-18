## MLST分析

参见[链接](https://github.com/sanger-pathogens/ariba/wiki/MLST-calling-with-ARIBA),没有多大用,直接使用mlst就好

## 耐药/毒力基因比对

> 耐药card,毒力vfdb_core

### 下载数据库

参考数据库如下

- [ARG-ANNOT](http://en.mediterranee-infection.com/article.php?laref=283%26titre=arg-annot). PMID: [24145532](http://www.ncbi.nlm.nih.gov/pubmed/24145532)
- [CARD](https://card.mcmaster.ca/). PMID: [23650175](http://www.ncbi.nlm.nih.gov/pubmed/23650175)
- [MEGARes](https://megares.meglab.org/) PMID: [27899569](http://www.ncbi.nlm.nih.gov/pubmed/27899569)
- [NCBI](https://www.ncbi.nlm.nih.gov/pathogens/isolates#/refgene/) BioProject: [PRJNA313047](https://www.ncbi.nlm.nih.gov/bioproject/?term=PRJNA313047)
- [plasmidfinder](https://cge.cbs.dtu.dk/services/PlasmidFinder/) PMID: [24777092](http://www.ncbi.nlm.nih.gov/pubmed/24777092)
- [resfinder](https://cge.cbs.dtu.dk//services/ResFinder/). PMID: [22782487](http://www.ncbi.nlm.nih.gov/pubmed/22782487)
- [VFDB](http://www.mgc.ac.cn/VFs/). PMID: [26578559](http://www.ncbi.nlm.nih.gov/pubmed/26578559)
- [SRST2](https://github.com/katholt/srst2)'s version of ARG-ANNOT. PMID: [25422674](http://www.ncbi.nlm.nih.gov/pubmed/25422674).
- [VirulenceFinder](https://cge.cbs.dtu.dk/services/VirulenceFinder/) PMID: [24574290](http://www.ncbi.nlm.nih.gov/pubmed/24574290).

where `reference_name` is one of: `argannot`, `card`, `ncbi`, `megares`, `plasmidfinder`, `resfinder`, `srst2_argannot`, `vfdb_core`, `vfdb_full`, `virulencefinder`.

```bash
ariba getref reference_name output_prefix
```

### 构建数据库

#### 使用参考fasta

```bash
ariba prepareref -f getref_out.fa -m getref_out.tsv prepareref.out
```

#### 使用自定义fasta

如果全是gene序列:

```bash
ariba prepareref --all_coding yes -f in.fasta out_dir
```

如果都是非编码序列:

```bash
ariba prepareref --all_coding no -f in.fasta out_dir
```

混合的情况必须提供metadata.tsv

```bash
ariba prepareref -f file.fa -m metadata.tsv out_dir
```

### 比对数据库

```bash
# 使用下机数据
ariba run ref reads_1.fq reads_2.fq output_dir
```

### 输出结果

```bash
ariba summary out in.report.1.tsv in.report.2.tsv ...
```

输出三个文件

- `out.csv`. 输出基因在各个样本中的存在情况.
- `out.phandango.{csv,tre}`. 将这两个文件拖到 [Phandango](http://jameshadfield.github.io/phandango/),可视化查看.