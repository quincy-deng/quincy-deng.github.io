**V2 使用配置文件**

```python
import re
import os
import sys

configfile:"config/spades.yaml"

rule all:
    input:
        "data/quast_results/report.html"

rule trimmomatic_qc:
    # noinspection PyRedundantParentheses
    input:
        fq1 = lambda wildcards: config[(wildcards.sample)][0],
        fq2 = lambda wildcards: config[(wildcards.sample)][1]
    output:
        fp = temp("data/trimmed/{sample}_forward_paired.fq.gz"),
        rp = temp("data/trimmed/{sample}_reverse_paired.fq.gz"),
        fup = temp("data/trimmed/{sample}_reverse_unpaired.fq.gz"),
        rup = temp("data/trimmed/{sample}_forward_unpaired.fq.gz")
    threads:1
    shell:
        "trimmomatic PE -phred33 -threads {threads} "
        "{input.fq1} {input.fq2} "
        "{output.fp} {output.fup} {output.rp} {output.rup} "
        "LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36 "

rule spades_assembly:
    input:
        f = "data/trimmed/{sample}_forward_paired.fq.gz",
        r = "data/trimmed/{sample}_reverse_paired.fq.gz"
    output:
        "data/fasta/{sample}.fasta"
    threads:5
    shell:
        "spades.py -t {threads} "
        "-1 {input.f} "
        "-2 {input.r} "
        "-o data/assembled/{wildcards.sample} 1>/dev/null;"
        "cp data/assembled/{wildcards.sample}/scaffolds.fasta {output}"

rule quast_qc:
    input:
        expand("data/fasta/{sample}.fasta",sample=config.keys())
    output:
        "data/quast_results/report.html"
    shell:
        "quast.py -o data/quast_results {input}"

```



**V1 在当前目录下寻找测序文件,QC并组装,最后评估组装质量.**

**查找fastq文件并软链接**

```bash
# 创建文件夹并软链接rawdata文件
find {path} -name *fq.gz|xargs -i ln -s {}
# 组装
conda run -n nosott snakemake --cores 15 -s /home/dengqiuyang/SFTP/NosoTT/scripts/assembly.rules
```

```python
import re
import os

x =os.listdir()

# 获取 Sample Name
suffix_re = re.compile('(\.|_)(|R)1\.f(|ast)q\.gz')

def get_sample(fq_name):
    x,_ = suffix_re.search(fq_name).span()
    return fq_name[:x]

samples = list(map(get_sample,filter(suffix_re.search,x)))


rule all:
    input:
        "quast_results/report.html"

rule trimmomatic_qc:
    output:
        "trimmed/{sample}_forward_paired.fq.gz",
        "trimmed/{sample}_reverse_paired.fq.gz"
    shell:
        "trimmomatic PE -phred33 "
        "{wildcards.sample}[._]*1.f*q.gz  {wildcards.sample}[._]*2.f*q.gz "
        "trimmed/{wildcards.sample}_forward_paired.fq.gz trimmed/{wildcards.sample}_forward_unpaired.fq.gz "
        "trimmed/{wildcards.sample}_reverse_paired.fq.gz trimmed/{wildcards.sample}_reverse_unpaired.fq.gz "
        "LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36"

rule spades_assembly:
    input:
        f = "trimmed/{sample}_forward_paired.fq.gz",
        r = "trimmed/{sample}_reverse_paired.fq.gz"
    output:
        "assembled/{sample}.fasta"
    shell:
        "spades.py "
        "-1 {input.f} "
        "-2 {input.r} "
        "-o assembled/{wildcards.sample} ;"
        "cp assembled/{wildcards.sample}/scaffolds.fasta assembled/{wildcards.sample}.fasta"

rule quast_qc:
    input:
        expand("assembled/{sample}.fasta",sample=samples)
    output:
        "quast_results/report.html"
    shell:
        "quast.py -o quast_results {input}"

```


