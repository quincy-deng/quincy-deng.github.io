**错误1:**

qt.qpa.xcb: could not connect to display 
qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "" even though it was found.
This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.

办法:卸载原本qt,再安装

```
conda remove qt
conda remove pyqt
conda install qt
conda install pyqt
```

**错误2**

qt.qpa.screen: QXcbConnection: Could not connect to display 
Could not connect to any X display.

办法:设置环境变量

运行->编辑配置->环境变量

```
DISPLAY=localhost:11.0
```

