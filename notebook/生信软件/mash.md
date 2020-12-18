## 通过 minhash 指纹快速进行菌种鉴定

### 原理图

![原理图](../../picture/13059_2016_997_Fig1_HTML.gif)

通过构建序列（基因组，原始reads）Kmer集合，并计算出kmer 的Hash集合， 然后通过杰卡德指数Jaccard Index 估计相似度， 即 A和B共享的Hash集合（Hash值可以通过Hash函数计算）与A和B所有的不同Hash集合的比值。 另外： 传统的DDH杂交算法也可以很好的进行菌株鉴定，但是速度没有使用MinHash快，下次介绍几种DDH相关工具。

[Mash](https://mash.readthedocs.io/en/latest/index.html)

### Simple distance estimation

```bash
mash dist genome1.fna genome2.fna
```

The results are tab delimited lists of Reference-ID, Query-ID, Mash-distance, P-value, and Matching-hashes:

```
genome1.fna   genome2.fna     0.0222766       0       456/1000
```

### Saving time by sketching first

```
mash sketch genome1.fna
mash sketch genome2.fna
mash dist genome1.fna.msh genome2.fna.msh
```

### Pairwise comparisons with compound sketch files

Sketch the first two genomes to create a combined archive, use `mash info` to verify its contents, and estimate pairwise distances:

```bash
mash sketch -o reference genome1.fna genome2.fna
mash info reference.msh
mash dist reference.msh genome3.fna
```

This will estimate the distance from each query (which there is one of) to each reference (which there are two of in the sketch file):

```
genome1.fna   genome3.fna     0       0       1000/1000
genome2.fna   genome3.fna     0.0222766       0       456/1000
```

### Querying read sets against an existing RefSeq sketch

Download the pre-sketched RefSeq archive (reads not provided here; 10x-100x coverage of a single genome with any sequencing technology should work):

[refseq.genomes.k21.s1000.msh](https://gembox.cbcb.umd.edu/mash/refseq.genomes.k21s1000.msh)

Plasmids also available:

[refseq.genomes+plasmids.k21.s1000.msh](https://gembox.cbcb.umd.edu/mash/refseq.genomes%2Bplasmid.k21s1000.msh) [refseq.plasmids.k21.s1000.msh](https://gembox.cbcb.umd.edu/mash/refseq.plasmid.k21s1000.msh)

Concatenate paired ends (this could also be piped to `mash` to save space by specifying `-` for standard input, zipped or unzipped):

```
cat reads_1.fastq reads_2.fastq > reads.fastq
```

Sketch the reads, using `-m 2` to improve results by ignoring single-copy k-mers, which are more likely to be erroneous:

```
mash sketch -m 2 reads.fastq
```

Run `mash dist` with the RefSeq archive as the reference and the read sketch as the query:

```bash
mash dist refseq.genomes.k21.s1000.msh reads.fastq.msh > distances.tab
```

Sort the results to see the top hits and their p-values:

```
sort -gk3 distances.tab | head
```

### Screening a read set for containment of RefSeq genomes

> (new in [Mash v2.0](https://github.com/marbl/Mash/releases))

If a read set potentially has multiple genomes, it can be “screened” against the database to estimate how well each genome is contained in the read set. We can use the [SRA Toolkit](https://www.ncbi.nlm.nih.gov/sra/docs/toolkitsoft/) to download ERR024951:

…and screen it against Refseq Genomes (link above), sorting the results:

```bash
mash screen refseq.genomes.k21s1000.msh ERR024951.fastq > screen.tab
sort -gr screen.tab | head
```

We see the expected organism, *Salmonella enterica*, but also an apparent contaminant, _Klebsiella pneumoniae_. The fields are \[identity, shared-hashes, median-multiplicity, p-value, query-ID, query-comment\]:

```
0.99957       991/1000        26      0       GCF_000841985.1\_ViralProj14228\_genomic.fna.gz   NC_004313.1 Salmonella phage ST64B, complete genome
0.99957       991/1000        24      0       GCF_002054545.1\_ASM205454v1\_genomic.fna.gz      \[57 seqs\] NZ_MYON01000010.1 Salmonella enterica strain BCW_4905 NODE\_10\_length\_152932\_cov_1.77994, whole genome shotgun sequence \[...\]
0.999522      990/1000        102     0       GCF_900086185.1\_12082\_4_85_genomic.fna.gz       \[51 seqs\] NZ_FLIP01000001.1 Klebsiella pneumoniae strain k1037, whole genome shotgun sequence \[...\]
0.999329      986/1000        24      0       GCF_002055205.1\_ASM205520v1\_genomic.fna.gz      \[72 seqs\] NZ_MYOO01000010.1 Salmonella enterica strain BCW_4904 NODE\_10\_length\_177558\_cov_3.07217, whole genome shotgun sequence \[...\]
0.999329      986/1000        24      0       GCF_002054075.1\_ASM205407v1\_genomic.fna.gz      \[88 seqs\] NZ_MYNK01000010.1 Salmonella enterica strain BCW_4936 NODE\_10\_length\_177385\_cov_3.78874, whole genome shotgun sequence \[...\]
0.999329      986/1000        24      0       GCF_000474475.1\_CFSAN001184\_01.0_genomic.fna.gz \[45 seqs\] NZ_AUQM01000001.1 Salmonella enterica subsp. enterica serovar Typhimurium str. CDC_2009K1158 isolate 2009K-1158 SEET1158_1, whole genome shotgun sequence \[...\]
0.999329      986/1000        24      0       GCF_000474355.1\_CFSAN001186\_01.0_genomic.fna.gz \[46 seqs\] NZ_AUQN01000001.1 Salmonella enterica subsp. enterica serovar Typhimurium str. CDC_2009K1283 isolate 2009K1283 (Typo) SEET1283_1, whole genome shotgun sequence \[...\]
0.999329      986/1000        24      0       GCF_000213635.1\_ASM21363v1\_genomic.fna.gz       \[2 seqs\] NC_016863.1 Salmonella enterica subsp. enterica serovar Typhimurium str. UK-1, complete genome \[...\]
0.999281      985/1000        24      0       GCF_001271965.1\_Salmonella\_enterica\_CVM\_N43825_v1.0_genomic.fna.gz      \[67 seqs\] NZ_LIMN01000001.1 Salmonella enterica subsp. enterica serovar Typhimurium strain CVM N43825 N43825\_contig\_1, whole genome shotgun sequence \[...\]
0.999281      985/1000        24      0       GCF_000974215.1_SALF-297-3.id2_v1.0_genomic.fna.gz      \[90 seqs\] NZ_LAPO01000001.1 
```

Salmonella enterica subsp. enterica serovar Typhimurium strain SALF-297-3 NODE_1, whole genome shotgun sequence \[...\]

Note, however, that multiple strains of _Salmonella enterica_ have good identity. This is because they are each contained well when considered independently. For this reason `mash screen` is not a true classifier. However, we can remove much of the redundancy for interpreting the results using the winner-take-all strategy (`-w`). And while we’re at it, let’s throw some more cores at the task to speed it up (`-p 4`):

```bash
mash screen -w -p 4 refseq.genomes.k21s1000.msh ERR024951.fastq > screen.tab
sort -gr screen.tab | head
```

The output is now much cleaner, with just the two whole genomes, plus phages (a lot of other hits to viruses and assembly contigs would appear further down):

```
0.99957       991/1000        24      0       GCF_002054545.1\_ASM205454v1\_genomic.fna.gz      \[57 seqs\] NZ_MYON01000010.1 Salmonella enterica strain BCW_4905 NODE\_10\_length\_152932\_cov_1.77994, whole genome shotgun sequence \[...\]
0.99899       979/1000        26      0       GCF_000841985.1\_ViralProj14228\_genomic.fna.gz   NC_004313.1 Salmonella phage ST64B, complete genome
0.998844      976/1000        101     0       GCF_900086185.1\_12082\_4_85_genomic.fna.gz       \[51 seqs\] NZ_FLIP01000001.1 Klebsiella pneumoniae strain k1037, whole genome shotgun sequence \[...\]
0.923964      190/1000        40      0       GCF_000900935.1\_ViralProj181984\_genomic.fna.gz  NC_019545.1 Salmonella phage SPN3UB, complete genome
0.900615      111/1000        100     0       GCF_001876675.1\_ASM187667v1\_genomic.fna.gz      \[137 seqs\] NZ_MOXK01000132.1 Klebsiella pneumoniae strain AWD5 Contig_(1-18003), whole genome shotgun sequence \[...\]
0.887722      82/1000 31      3.16322e-233    GCF_001470135.1\_ViralProj306294\_genomic.fna.gz  NC_028699.1 Salmonella phage SEN34, complete genome
0.873204      58/1000 22      1.8212e-156     GCF_000913735.1\_ViralProj227000\_genomic.fna.gz  NC_022749.1 Shigella phage SfIV, complete genome
0.868675      52/1000 57      6.26251e-138    GCF_001744215.1\_ViralProj344312\_genomic.fna.gz  NC_031129.1 Salmonella phage SJ46, complete genome
0.862715      45/1000 1       1.05185e-116    GCF_001882095.1\_ViralProj353688\_genomic.fna.gz  NC_031940.1 Salmonella phage 118970_sal3, complete genome
0.856856      39/1000 21      6.70643e-99     GCF_000841165.1\_ViralProj14230\_genomic.fna.gz   NC_004348.1 Enterobacteria phage ST64T, complete genome
```

