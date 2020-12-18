**SSH远程连接**

文件位置~/.ssh/config

```json
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

Host fuwuqi
  HostName 172.20.70.157
  User dengqiuyang

```

