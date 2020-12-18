## 安装samba服务

```bash
yum install -y samba
```

## 配置config

```bash
# 备份
cp /etc/samba/smb.conf  /etc/samba/smb.conf.bak
# 编辑smb.conf 
[global]
         workgroup = WORKGROUP
         server string = Ted Samba Server %v
         netbios name = TedSamba
         security = user
         map to guest = Bad User
         passdb backend = tdbsam

[Faraway]
         comment = project development directory
         path = /home/dengqiuyang/SFTP
         valid users = dengqiuyang
         write list = dengqiuyang
         printable = no
         create mask = 0644
         directory mask = 0755
# 修改文件夹权限
chmod 777 SFTP
# 测试config文件是否可用
testparm
```

## 添加用户

```bash
sudo smbpasswd -a dengqiuyang # 密码可以不同
```

## 关闭SElinux

```
sudo setenforce 0 # 临时关闭  1开启
```

## 启动samba服务

```
sudo systemctl start smb
```

## windows下访问

```
\\172.20.70.157
```

## windows共享文件夹

1. 由于windows自带smb服务,在程序和功能中打开.
2. 选择文件夹属性/共享/设置共享用户和权限.这一步可能需要启用网络发现和文件共享.
3. 在另外一台电脑访问该电脑ip,输入共享电脑的的账号和密码.
4. (optional)映射到盘符,方便以后访问.