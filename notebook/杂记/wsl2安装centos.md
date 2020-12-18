### xu启用wsl

“控制面板”——双击“程序和功能”——点击左侧的“启用或关闭 Windows 功能”——勾中“适用于 Linux 的 Windows 子系统”——点击“确定”按钮

windows商店安装wsl

### 下载 CentOS Docker 镜像

1.  访问 CentOS 的官方 Docker 镜像网站：[CentOS Cloud SIG image repository.](https://github.com/CentOS/sig-cloud-instance-images)；
2.  切换到自己想要的分支，比如：[CentOS-7-x86\_64](https://github.com/CentOS/sig-cloud-instance-images/tree/CentOS-7-x86_64)；
3.  进入 docker 目录，下载centos-\*-docker.tar.xz文件，比如：[centos-7-x86\_64-docker.tar.xz](https://github.com/CentOS/sig-cloud-instance-images/raw/CentOS-7-x86_64/docker/centos-7-x86_64-docker.tar.xz)；

### 安装 Chocolatey

[Chocolatey](https://chocolatey.org/) 是 Win­dows 环境下的包管理器，其作用等同于 Mac OS 的 Brew，Ubuntu 的 apt，Cen­tOS 的 yum。

管理员权限的 Pow­er­shell 中执行下列命令：

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

### 安装 LxRunOffline

```powershell
choco install lxrunoffline
```
重启powershell

### 使用 LxRunOffline 安装 CentOS

```powershell
LxRunOffline install -n centos -d 目标路径 -f centos-7-x86_64-docker.tar.xz
```

其中：

- -n 是安装的系统名称，可自定义；
- -d 是安装系统的目录；
- -f 是之前下载的镜像路径；

接下来,wsl自动添加了centos,完成.