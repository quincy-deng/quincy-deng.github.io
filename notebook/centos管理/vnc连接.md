> 其实并没有完全成功,现在root正常,非root可连但是黑屏
>
> 端口590N,一般5901	

### 安装tigervnc

```bash
yum -y install tigervnc-server
```

### 创建配置文件

```bash
sudo cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
```

**vncserver@:1.service** 为 **5901** 端口，使用 **vncviewer** 连接时，需要连接的地址为 **[192.168.xxx.xxx:5901](http://192.168.xxx.xxx:5901/)**  
**vncserver@:2.service** 为 **5902** 端口，使用 **vncviewer** 连接时，需要连接的地址为 **[192.168.xxx.xxx:5902](http://192.168.xxx.xxx:5902/)**  
以此类推，请按需要进行配置，默认使用**5901**即可。

### 编辑配置文件

编辑对应配置文件 **vncserver@:1.service** 、 **vncserver@:2.service** 、**vncserver@:3.service** ……

```bash
sudo vi /etc/systemd/system/vncserver@:1.service
```

**vncserver@:1.service** 此文件 在终端中显示可能会多一个 **“\\”** ， 实际上是同一个文件，请不要在意。

* * *

*   文件\[Service\]部分，内容如下

```bash
[Service]
Type=forking

# Clean any existing files in /tmp/.X11-unix environment
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
ExecStart=/usr/sbin/runuser -l <USER> -c "/usr/bin/vncserver %i"
PIDFile=/home/<USER>/.vnc/%H%i.pid
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
```

将其中的 **< USER >** 修改为远程登录的账号，例如 **dengqiuyang** , 共计修改两处，其余未做任何改动

*   改动后如下

```bash
[Service]
Type=forking

# Clean any existing files in /tmp/.X11-unix environment
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
ExecStart=/usr/sbin/runuser -l dengqiuyang -c "/usr/bin/vncserver %i"
PIDFile=/home/dengqiuyang/.vnc/%H%i.pid
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'

```

### 设置tigervnc密码

注：需要在配置文件中所配置的**远程登录用户下设置对应的vnc密码**  
例如：在配置文件中 将远程登录用户设置为 **centos** ，那么需要切换到 **centos** 下执行命令


*   设置vnc远程登录的密码

```bash
vncpasswd
```

### 开启5901端口

### 启动vnc服务并设置开机启动服务

```bash
sudo systemctl daemon-reload
sudo systemctl enable vncserver@\:1.service
sudo systemctl start vncserver@\:1.service
```

### 远程控制连接

使用vncviewer连接.

### vncserver使用

```bash
# 打开服务,前面systemctl已打开
vncserver
# 查看已打开的服务
vncserver -list
vncserver -kill :1
```