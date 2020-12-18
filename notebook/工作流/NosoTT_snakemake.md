调试好的

```python
import os
import shutil
import pandas as pd
import datetime

samples = ['3','6','9']

casedate = r'/home/dengqiuyang/Transmission_DataFile/casedate/casedatabase.csv'
fasta_dir = r'/home/dengqiuyang/Transmission_DataFile/fasta'
basedir =  r'/home/dengqiuyang/Transmission_DataFile/'
temp_dir = r'/home/dengqiuyang/fasta_temp'

if not os.path.exists(temp_dir):
    os.mkdir(temp_dir)

def copy_file(path,fastas):
    ls = os.listdir(path)
    for i in ls:
        c_path = os.path.join(path, i)
        if os.path.isdir(c_path):
            del_file(c_path)
        else:
            os.remove(c_path)
    [shutil.copy(i,path) for i in fastas[1:]]

def generate_csv(casedata):
    dates = ['Admdate','Sampdate','Chgdate']

    # casedata = pd.read_csv(casedate,names=['item','name','Admdate','Sampdate','Chgdate'])
    baseDate = min(casedata['Sampdate'])

    for ind,row in casedata.iterrows():
        bDate = datetime.datetime.strptime(baseDate,"%Y-%m-%d")
        Adate,Sdate,Cdate = [datetime.datetime.strptime(row[i],"%Y-%m-%d") for i in dates]
        casedata['Admdate'][ind], casedata['Sampdate'][ind],casedata['Chgdate'][ind] = \
            (Adate - bDate).days,(Sdate - bDate).days, (Cdate - bDate).days

    dates = pd.DataFrame({'A':casedata['name'],'B':casedata['Sampdate']})
    hosts = pd.DataFrame({'A':casedata['name'],'B':casedata['item']})
    hostTimes = pd.DataFrame({'A':casedata['item'],'B':casedata['Admdate'],'C':casedata['Chgdate']})

    try:
        dates.to_csv('{}dates.csv'.format(basedir),index=False,header=False)
        hosts.to_csv('{}hosts.csv'.format(basedir),index=False,header=False)
        hostTimes.to_csv('{}hostTimes.csv'.format(basedir),index=False,header=False)
    except IOError as e:
        print("Save datefile failed!:%s", e)
    
    print("Generate date file success")

# 数据筛选
df = pd.read_csv(casedate,names=['item','name','Admdate','Sampdate','Chgdate'])
df = df[df['item'].isin(samples)]
fastas = [fasta_dir+"/"+i+".fasta" for i in df['name']]

# 准备对应数据
copy_file(temp_dir,fastas)
generate_csv(df)
ref = fastas[0]

# 相关命令
generateXml = "python {}SCOTTI_generate_xml.py  -o scotti -d {}dates.csv -ho {}hosts.csv -ht {}hostTimes.csv -ov -m {} -n 100000 -f ".format(basedir,basedir,basedir,basedir,len(samples) + 2)

generateTree = "java -cp {}SCOTTI.v2.0.1.jar:{}jblas-1.2.3.jar:{}guava-15.0.jar:{}beast.jar beast.app.beastapp.BeastMain -overwrite ".format(basedir,basedir,basedir,basedir)

generateJpg = "python {}Make_transmission_tree_alternative.py".format(basedir)


# 写入规则   
rule all:
    input:
        "disease_direct_transmissions.jpg",
        "disease_indirect_transmissions.jpg"

rule parsnp_align:
    output:
        "parsnp/parsnp.xmfa"
    shell:
        "parsnp -r {ref} -o parsnp -d {temp_dir} -c -x 1>/dev/null"

rule harvesttools_transform:
    input:
        "parsnp/parsnp.xmfa"
    output:
        "parsnp/parsnp.fasta"
    shell:
        "harvesttools -x {input} -M {output};"
        "sed -i 's/.ref//g' parsnp/parsnp.fasta;"
        "sed -i 's/.fasta//g' parsnp/parsnp.fasta"

rule generate_xml:
    input:
        "parsnp/parsnp.fasta"
    output:
        temp("parsnp"),
        temp("scotti.xml")
    shell:
        "{generateXml} parsnp/parsnp.fasta"

rule generate_tree:
    input:
        "scotti.xml"
    output:
        temp("scotti.trees"),
        temp("scotti.log"),
        temp("scotti.xml.state")
    shell:
        "{generateTree} {input}"

rule generate_jpg:
    input:
        "scotti.trees"
    output:
        "disease_direct_transmissions.jpg",
        "disease_indirect_transmissions.jpg"
    shell:
        "{generateJpg} --input {input}  --outputF disease --minValue 0.2"

```

