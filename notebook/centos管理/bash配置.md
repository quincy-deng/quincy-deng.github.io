```bash
# 终端配色
    PS1='\[\e[33;1m\]\u@\h \[\e[35;1m\]\t \[\e[34;1m\]`pwd`\n\[\e[32;1m\]$ \[\e[0m\]'

# 命令别称
    alias les="less -S"
    alias ll="ls -lh"
    alias ls="ls --color=auto"
    alias wl="wc -l"
    alias off="conda deactivate"
    alias tp="top -u $USER"

# 中文显示
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8

# conda 环境
	alias abricate_envs="conda activate abricate"
    alias ariba_envs="conda activate ariba"
    alias bwa_envs="conda activate bwa"
    alias mash_envs="conda activate mash"
    alias mlst_envs="conda activate mlst"
    alias mumer_envs="conda activate mumer"
    alias prokka_envs="conda activate prokka"
    alias quast_envs="conda activate quast"
    alias raxml_envs="conda activate raxml"
    alias roary_envs="conda activate roary"
    alias samtools_envs="conda activate samtools"
    alias snippy_envs="conda activate snippy"
    alias spades_envs="conda activate spades"
    alias panito_envs="conda activate panito"
    alias gubbins_envs="conda activate gubbins"
```

