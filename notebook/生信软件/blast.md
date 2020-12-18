## 构建参考序列

```bash
makeblastdb -in hvct.fasta -dbtype nucl -parse_seqids -out ./index
```

## 比对

```bash
blastn -query in.fa -db ./index -evalue 1e-6  -num_threads 6 -out out_file
```

```python
# 筛选
    if genes:
        command = ['tblastn',
                   '-db_gencode', '11',  # bacterial translation table
                   '-seg', 'no']         # don't filter out low complexity regions
    else:
        command = ['blastn', '-task', 'blastn']
    command += ['-db', database, '-query', query, '-num_threads', str(threads), '-outfmt',
                '6 qseqid sseqid qstart qend sstart send evalue bitscore length pident qlen qseq '
                'sseq']
coverage = qseq/qlen 
coverage >0.9
pident > 0.8
```

**format 6**

| 1.   | `qseqid`   | query (e.g., unknown gene) sequence id                       |
| :--- | ---------- | ------------------------------------------------------------ |
| 2.   | `sseqid`   | subject (e.g., reference genome) sequence id                 |
| 3.   | `pident`   | percentage of identical matches                              |
| 4.   | `length`   | alignment length (sequence overlap)                          |
| 5.   | `mismatch` | number of mismatches                                         |
| 6.   | `gapopen`  | number of gap openings                                       |
| 7.   | `qstart`   | start of alignment in query                                  |
| 8.   | `qend`     | end of alignment in query                                    |
| 9.   | `sstart`   | start of alignment in subject                                |
| 10.  | `send`     | end of alignment in subject                                  |
| 11.  | `evalue`   | [expect value](https://sites.google.com/site/wiki4metagenomics/tools/blast/evalue) |
| 12.  | `bitscore` | [**bit score**](https://sites.google.com/site/wiki4metagenomics/tools/blast/evalue) |

## Define your own output format

*by adding the option -outfmt, as for example:***
**

```
 -outfmt "6  qseqid sseqid evalue"
```

***supported format specifiers are:***
`qseqid` Query Seq-id
`qgi    `Query GI
`qacc    `Query accesion
`qaccver  `Query accesion.version
`qlen    `Query sequence length
`sseqid   `Subject Seq-id
`sallseqid `All subject Seq-id(s), separated by a ';'
`sgi     `Subject GI
`sallgi   `All subject GIs
`sacc    `Subject accession
`saccver  `Subject accession.version
`sallacc  `All subject accessions
`slen    `Subject sequence length
`qstart   `Start of alignment in query
`qend    `End of alignment in query
`sstart   `Start of alignment in subject
`send    `End of alignment in subject
`qseq    `Aligned part of query sequence
`sseq    `Aligned part of subject sequence
`evalue   `Expect value
`bitscore `Bit score
`score   `Raw score
`length   `Alignment length
`pident   `Percentage of identical matches
`nident   `Number of identical matches
`mismatch `Number of mismatches
`positive `Number of positive-scoring matches
`gapopen  `Number of gap openings
`gaps    `Total number of gaps
`ppos    `Percentage of positive-scoring matches
`frames   `Query and subject frames separated by a '/'
`qframe   `Query frame
`sframe   `Subject frame
`btop    `Blast traceback operations (BTOP)
`staxids  `Subject Taxonomy ID(s), separated by a ';'
`sscinames `Subject Scientific Name(s), separated by a ';'
`scomnames `Subject Common Name(s), separated by a ';'
`sblastnames `Subject Blast Name(s), separated by a ';'  (in alphabetical order)
`sskingdoms `Subject Super Kingdom(s), separated by a ';'   (in alphabetical order)
`stitle    `Subject Title
`salltitles `All Subject Title(s), separated by a '<>'
`sstrand  `Subject Strand
`qcovs   `Query Coverage Per Subject
`qcovhsp  `Query Coverage Per HSP