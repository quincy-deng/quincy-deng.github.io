**1. wrapped C/C++ object of type QTableWidget has been deleted**

 ![image-20200915090150084](../../picture/image-20200915090150084.png)

因为QTableWidget()是QDialog的子控件,原本case_dialog(Dialog)不是全局变量,导致槽函数调用case_table出错,加上self就行了.