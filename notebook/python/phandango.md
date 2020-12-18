1. 生成符合**ariba summary**的输入数据文件.
    本例子中把每种KL型的基因生成一个文件
    ```python
    from Bio import SeqIO
    cols_str = r"#ariba_ref_name	ref_name	gene	var_only	flag	reads	cluster	ref_len	ref_base_assembled	pc_ident	ctg	ctg_len	ctg_cov	known_var	var_type	var_seq_type	known_var_change	has_known_var	ref_ctg_change	ref_ctg_effect	ref_start	ref_end	ref_nt	ctg_start	ctg_end	ctg_nt	smtls_total_depth	smtls_nts	smtls_nts_depth	var_description	free_text"
    cols = cols_str.split("\t")
    KLs = ["KL2","KL3","KL14","KL77","KL49"]
    KL_gbff = r"/home/dengqiuyang/lyb24_hvct/Kaptive-master/reference_database/Acinetobacter_baumannii_k_locus_primary_reference.gbk"

    def genbank_parse(genbank):
        KLs_genes = []
        for record in SeqIO.parse(genbank, 'genbank'):
            KL_genes = []
            KL_genes.append(str(record.name))
            for feature in record.features:
                if 'gene' in feature.qualifiers:
                    gene = feature.qualifiers['gene'][0]
                    seq_len = len(feature.extract(record.seq))
                    KL_genes.append([gene,seq_len])
            KLs_genes.append(KL_genes)

        return KLs_genes

    KLs_gens = genbank_parse(KL_gbff)

    fake = ["." for i in range(18)]
    for KL in KLs_gens:
        KL_name = KL[0]
        with open("{}.tsv".format(KL_name),'w') as o:
            o.write("\t".join(cols))
            genes = KL[1:]
            for n,(gene,seq_len) in enumerate(genes):
                line = [gene,gene,1,0,27,n,gene,seq_len,seq_len,100,gene+"ctg{}".format(n),seq_len,100]+fake
                line = [str(i) for i in line]
                o.write("\n" + "\t".join(line))
    ```

2. 获得phandango所需文件

   ```python
   ariba summary out2 KL{2,3,14,49,77}.tsv
   ```