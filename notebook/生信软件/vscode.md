

## 远程连接虚拟机或者服务器

```json
# 注意不能有注释
## 必须写localhost,127.0.0.1不行
Host NAT_connection
  HostName localhost
  User dengqiuyang
  Port 8080

Host host_only
  HostName 192.168.56.104
  User dengqiuyang

Host aliyun
  HostName 38.108.106.87
  User dengqiuyang
  Port 8090
```

## 插件

### sftp

```json
{
    "name": "My Server",
    "host": "192.168.56.104",
    "protocol": "sftp",
    "port": 22,
    "username": "dengqiuyang",
    "password":"bac0305",
    "remotePath": "/home/dengqiuyang/lyb24_hvct",
    "uploadOnSave": true
}
```

