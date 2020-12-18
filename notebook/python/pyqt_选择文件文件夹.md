*   选择文件夹：
    
    ```python
    directory = QtWidgets.QFileDialog.getExistingDirectory(self, "getExistingDirectory", "./") 
    
    ```
    
*   选择文件：
    
    ```python
    directory = QtWidgets.QFileDialog.getOpenFileName(self,
                  "getOpenFileName","./",
                  "All Files (*);;Text Files (*.txt)") 
    ```
    
*   选择多个文件
    
    ```python
    directory = QtWidgets.QFileDialog.getOpenFileNames(self, 
                "getOpenFileNames", "./",
                "All Files (*);;Text Files (*.txt *.tab)") 
    
    ```
    
*   设置保存文件名称路径
    
    ```python
    directory = QtWidgets.QFileDialog.getSaveFileName(self, 
                  "getSaveFileName","./",
                  "All Files (*);;Text Files (*.txt)") 
    ```

