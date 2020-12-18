## Calling SNP差异基因

> 与参考基因组相比

### 比对并排列

```shell
$ bwa index ST17.fasta
bwa mem -t 15 -R '@RG\tID:TAP1\tSM:TAP1' ST17.fasta \
> TAP1_R1_paired.fastq.gz TAP1_R2_paired.fastq.gz | \
> samtools view -bS - | samtools sort - TAP1.sorted
$ samtools index TAP1.sorted.bam
```

### optional 绘制测序 reads 的覆盖度

```shell
$ samtools stats TAP1.sorted.bam | grep "maximum length"
$ samtools stats TAP1.sorted.bam | grep "insert size"
$ samtools stats TAP1.sorted.bam | grep "^COV" > TAP1.coverage.txt
```

```R
> library(ggplot2)
> cov=read.table("TAP1.coverage.txt", sep="\t")
> cov[1,]
> ggplot(cov, aes(x=V3, y=V4)) + geom_bar(stat="identity") + xlab("Coverage") + ylab("Count")
```

### 寻找差异基因

用vcfsamplediff了解SNPs差异的基因，可以知道是那些gene/CDS里面发生了变化，还可以根据定位看突变是否是同义突变。如果snps数量接近无法区分，但是定位的基因有差异，那么菌株的溯源还需要进一步考虑。


```shell

$ vcfsamplediff SAME PATIENT1 TAP1 compare_tap1.vcf  | vcffilter -f "QUAL / AO > 10" | vcffilter -f "NS = 2" | vcffilter -f "! ( SAME = germline ) " > tap_differences.vcf
$ bedtools intersect -a ST17.gff -b tap_differences.vcf | awk -F";" '{print NR, $2}' OFS="\t"
```

结果输出：

```shell
1   gene=yfiR
2   inference=ab initio prediction:Prodigal:2.60,similar to AA sequence:RefSeq:YP_261793.1,protein motif:TIGRFAMs:TIGR03756,protein motif:Pfam:PF06834.5
3   inference=ab initio prediction:Prodigal:2.60,similar to AA sequence:RefSeq:YP_261793.1,protein motif:TIGRFAMs:TIGR03756,protein motif:Pfam:PF06834.5
4   gene=trbL
5   inference=ab initio prediction:Prodigal:2.60,similar to AA sequence:RefSeq:YP_001349879.1,protein motif:Cdd:COG3481,protein motif:TIGRFAMs:TIGR03760,protein motif:Pfam:PF07514.5
```

