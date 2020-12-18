周老师组分析方法

### SNP analysis

#### reads

map:CLC Genomics Workbench v6.05

> 会产生MGEs (prophages and genomic islands) and repeats

filter:CLC Genomics Workbench

1. their quality score was <30
2. the neighbourhood quality was <30
3. the minimum variant frequency was <35%
4. the minimum coverage was <1
5. they were only detected on a single strand

#### assembly genome

Mauve

### Genome analysis: genomic islands, prophages and plasmids

**genomic islands**

Fragments >5 kb that were absent in at least one genome were detected by BLAST

**prophages**

PHAST

**plasmid**

 BLASTn,比对给定质粒.

BRIG绘图

### Core-genome phylogenetic analysis

Mauve

Fragments (≥500 bp) shared by all genomes were collected and then concatenated. 

A maximum likelihood (ML) phylogeny was estimated by RAxML v7.2.8 with 1000 bootstrap replications under the general time-reversible model with Gamma correction (GTR+G).

参见文章:

[产志贺毒素的大肠O104](The mosaic genome structure and phylogeny
of Shiga toxin-producing Escherichia coli O104:H4 is driven by short-term
adaptation.)

[鲍曼ST457高毒力](https://academic.oup.com/cid/article/67/suppl_2/S179/5181271)