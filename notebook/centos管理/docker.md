## 离线安装docker

[离线安装docker](https://blog.csdn.net/u013058742/article/details/80075633)

> 文件放置坚果云上了

### windows安装问题

1. Hardware assisted virtualization and data execution protection must be enabled in the BIOS

   虚拟化已经启动还是不行的解决办法

   ```
   bcdedit /set hypervisorlaunchtype auto
   ```

2. wsl2不完整

## 离线安装镜像

### 无网络环境下使用docker加载镜像

```bash
# 先从一个有网络的电脑下载docker镜像
  docker pull centos
# 保存镜像到本地文件
  docker save -o centos_image.docker centos
# 把镜像拷贝到无网络的电脑，然后通过docker加载镜像即可。
  docker load -i centos_image.docker
```

## docker使用

```bash
# 查看镜像
docker images
# 查看容器
docker ps
docker ps -a
# 进入正在运行的容器
docker exec contain_ID /bin/bash
docker exec -it  f94d2c317477 /bin/bash
# 根据Dockerfile构建
docker build -t nosott .

# 删除所有停止的容器
docker rm -v $(docker ps -a -q -f status=exited)
docker container prune
# 删除镜像
docker rmi 1c4973ba37b9
# 删除所有镜像(慎重)
docker rmi $(docker images -q)

# 在运行docker容器时可以加如下参数来保证每次docker服务重启后容器也自动重启：
docker run --restart=always
# 如果已经启动了则可以使用如下命令：
docker update --restart=always <CONTAINER ID>
```

### Dockerfile实例

根据conda配置文件创建

```
FROM continuumio/anaconda3

ADD nosott.yaml /tmp/environment.yml
ENV LANG C.UTF-8
ENV SHELL /bin/bash
RUN conda env create -f /tmp/environment.yml && \
    conda clean --all -y

RUN echo "conda activate nosott" >> ~/.bashrc
ENV PATH /opt/conda/envs/nosott/bin:${PATH}
```

命令

```
# Halo 博客
docker run --rm -it -d --name halo -p 8090:8090  -v ~/.halo:/root/.halo ruibaby/halo
# Rstudio
docker run --rm -it -d -p 8888:8787 -v ~/Bacterial-in-hospital/R:/home/rstudio/script -e PASSWORD=password davetang/rstudio_biocasia
# jupyter
docker run --rm -itd --name notebook -p 8787:8888 -e JUPYTER_ENABLE_LAB=yes -v "$PWD":/home/jovyan/work jupyter/scipy-notebook
docker logs --tail 3 notebook //查看token

```

