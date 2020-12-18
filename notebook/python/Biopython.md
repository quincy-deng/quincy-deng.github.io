## SeqIO读取/写入/提取序列

**从genebank文件中提取基因的核酸序列\氨基酸序列,以及contig序列.**

```python
# 核心代码
from Bio import SeqIO
# 读取注释文件
for record in SeqIO.parse(genbank, 'genbank'):
    for feature in record.features:
        if 'gene' in feature.qualifiers and feature.qualifiers['gene'][0] == gene_name:
            pass
    
# 写入序列       
res = open(args.out_file, "w")
for record in SeqIO.parse(input.fasta, 'fasta'):
    if record.id == "ST823_GCF_900495225.fna":
        continue
    SeqIO.write(record,res,'fasta')        
        
# 序列
# contig
record.seq
# contig name
record.name

# feature
feature.type:'source','CDS','misc_feature','rRNA','',...
# gene seq
feature.extract(record.seq)
# gene acid seq
feature.extract(record.seq).translate(table=11)
# gene name 
feature.qualifiers['gene'][0]
# 注释
feature.qualifiers['product'][0]
feature.qualifiers['EC_number'][0]
...
```

:arrow_down:完整代码

```python
# 使用Bio包解析bek文件,提取序列
from Bio import SeqIO

genes = ['arnC','arnD','arnT','arnE','arnF']
genebank = r"/home/dengqiuyang/01.KP/肺炎克雷伯ATCC13883/GCF_000742135.1_ASM74213v1_genomic.gbff"

def main():
    with open("genes.fasta",'w') as o:
        for gene in genes:
            o.write(">"+gene+"\n")
            seq = parse_genbank(genebank,gene)
            o.write(add_line_breaks_to_sequence(seq,60))


def parse_genbank(genbank,gene_name):
    seq = ""
    for record in SeqIO.parse(genbank, 'genbank'):
            for feature in record.features:
                # if feature.type == 'CDS':
                if 'gene' in feature.qualifiers and feature.qualifiers['gene'][0] == gene_name:
                        seq = feature.extract(record.seq)
    return str(seq)

def add_line_breaks_to_sequence(sequence, length):
    """Wraps sequences to the defined length. All resulting sequences end in a line break."""
    seq_with_breaks = ''
    while len(sequence) > length:
        seq_with_breaks += sequence[:length] + '\n'
        sequence = sequence[length:]
    if sequence:
        seq_with_breaks += sequence
        seq_with_breaks += '\n'
    return seq_with_breaks

if __name__ == '__main__':
    main()
```

**gbk读取,写入fasta或者gbk**

```python
# 相当于每一个contig都写入一个fasta
for record in SeqIO.parse(gbff,'genbank'):
    SeqIO.write(record,'KLs_fasta/{}.fasta'.format(record.id),'fasta')

# 每一个contig写一个gbk
for record in SeqIO.parse(gbff,'genbank'):
    SeqIO.write(record,'KLs_fasta/{}.gbk'.format(record.id),'genbank')

# 写一个fasta
records = SeqIO.parse(gbff,'genbank')
SeqIO.write(records,'KLs.fasta','fasta')

```

## 序列相似性

```python
from Bio import pairwise2 as pw2
t_len = len(first_seq)
global_align = pw2.align.globalxx(first_seq, second_seq)
# 局部比对
# local_align = pw2.align.localxx(first_seq, second_seq)
matched = global_align[0][2]
percent_match = (matched / t_len) * 100
```

