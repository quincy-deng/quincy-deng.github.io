# 使用方法

### SSH 端口转发搭建 Jupyter notebook 服务器

#### Jupyter 配置

**生成密钥**

```python
>> from notebook.auth import passwd
>> passwd()
Enter password: 
Verify password: 
'sha1:673a8456a8e8:4377bd9ee8dc33d4cb5a2011f7a89643de15c11c'
```

**设置配置文件**

自行创建一个配置文件，然后在运行 Jupyter notebook 的时候动态加载配置信息。

创建配置文件，可以取名为 `jupyter_config.py`

配置内容如下：

```python
c.NotebookApp.ip = 'localhost' # 指定
c.NotebookApp.open_browser = False # 关闭自动打开浏览器
c.NotebookApp.port = 8888 # 端口随意指定
c.NotebookApp.password = u'sha1:d8334*******' # 复制前一步生成的密钥
```

**启动 Jupyter 服务器**

```bash
jupyter notebook --config=jupyter_config.py
```

