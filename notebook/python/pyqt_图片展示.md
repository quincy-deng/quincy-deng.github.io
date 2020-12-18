



```python
from PyQt5.QtWidgets import (QWidget,QApplication,QHBoxLayout,QVBoxLayout,QScrollArea,QGridLayout,
                             QPushButton,QFileDialog,QMessageBox,QLabel,QDialog)
from PyQt5.QtCore import Qt,pyqtSignal,QSize
from PyQt5.QtGui import QPixmap,QFont
import os
import sys
import time


class Main_win(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle('主界面')
        self.main_ui()
        self.btn_open.clicked.connect(self.show_fig)

    def main_ui(self):
        layout = QHBoxLayout()

        self.btn_open = QPushButton('显示结果')
        self.btn_open.setFixedSize(100,30),self.btn_open.setFont(QFont('consolas'))
        layout.addWidget(self.btn_open)

        self.setLayout(layout)

    def show_fig(self):
        x = img_viewed()
        # x.gridLayout
        x.show()
        x.exec_()

class img_viewed(QDialog):
    def __init__(self,parent =None):
        super().__init__()
        self.dialog_ui()
        self.open_file_pushbutton.clicked.connect(self.choose_dir)
        self.start_file_pushbutton.clicked.connect(self.start_img_viewer)

    def dialog_ui(self):
        layout = QVBoxLayout()
        self.scroll_ares_images = QScrollArea()
        self.scroll_ares_images.setWidgetResizable(True)

        self.scrollAreaWidgetContents = QWidget()

        # 进行网络布局
        self.gridLayout = QGridLayout(self.scrollAreaWidgetContents)
        self.scroll_ares_images.setWidget(self.scrollAreaWidgetContents)
        self.scroll_ares_images.setFixedSize(960,500)
        
        wid = QWidget()
        layout1 = QHBoxLayout(wid)
        layout1.addStretch(2)
        self.open_file_pushbutton =QPushButton('打开文件夹')
        self.open_file_pushbutton.setFixedSize(100,30),self.open_file_pushbutton.setFont(QFont('consolas'))
        layout1.addWidget(self.open_file_pushbutton)
        layout1.addStretch(1)
        self.start_file_pushbutton = QPushButton('开始')
        self.start_file_pushbutton.setFixedSize(100,30),self.start_file_pushbutton.setFont(QFont('consolas'))
        layout1.addWidget(self.start_file_pushbutton)
        layout1.addStretch(2)

        layout.addWidget(wid)
        layout.addWidget(self.scroll_ares_images)
        self.setLayout(layout)

        #设置图片的预览尺寸；
        self.displayed_image_size = 100
        self.col = 0
        self.row =0
        self.initial_path =None

    def choose_dir(self):
        file_path = QFileDialog.getExistingDirectory(self, '选择文文件夹', '/')
        if file_path ==None:
            QMessageBox.information(self,'提示','文件为空，请重新操作')
        else:
            self.initial_path =file_path

    def start_img_viewer(self):
        file_path = self.initial_path
        if self.initial_path:
            png_list = list(i for i in os.listdir(file_path) if str(i).endswith('.{}'.format('png')))
            num = len(png_list)
            if num !=0:
                for i in range(num):
                    image_id = str(file_path + '/' + png_list[i])
                    pixmap = QPixmap(image_id)
                    self.addImage(pixmap, image_id)
                    QApplication.processEvents()
            else:
                QMessageBox.warning(self,'错误','生成图片文件为空')
                self.event(exit())
        else:

            QMessageBox.warning(self, '错误', '文件为空，请稍后')

    def addImage(self, pixmap, image_id):
        #图像法列数
        self.max_columns = self.get_nr_of_image_columns()
        clickable_image = QClickableImage(pixmap, image_id)
        self.gridLayout.addWidget(clickable_image, self.row, self.col)
        if self.col < self.max_columns:
            self.col =self.col +1
        else:
            self.col =0
            self.row +=1

    def get_nr_of_image_columns(self):
        #展示图片的区域
        scroll_area_images_width = 960
        if scroll_area_images_width > self.displayed_image_size:
            pic_of_columns = scroll_area_images_width // self.displayed_image_size  #计算出一行几列；
        else:
            pic_of_columns = 1
        return pic_of_columns


class QClickableImage(QWidget):
    def __init__(self,pixmap =None,image_id = '',width =100,height =100):
        super().__init__()
        self.layout =QVBoxLayout()
        self.label1 = QLabel()
        # self.label1.setObjectName('label1')
        self.lable2 =QLabel()
        # self.lable2.setObjectName('label2')
        self.width =width
        self.height = height
        self.pixmap =pixmap

        if self.width and self.height:
            self.resize(self.width,self.height)
        if self.pixmap:
            pixmap = self.pixmap.scaled(QSize(self.width,self.height),Qt.KeepAspectRatio,Qt.SmoothTransformation)
            self.label1.setPixmap(pixmap)
            self.label1.setAlignment(Qt.AlignCenter)
            self.layout.addWidget(self.label1)
        if image_id:
            self.image_id =image_id
            self.lable2.setText(image_id)
            self.lable2.setAlignment(Qt.AlignCenter)
            ###让文字自适应大小
            self.lable2.adjustSize()
            self.layout.addWidget(self.lable2)
        self.setLayout(self.layout)


if __name__ =='__main__':
    app =QApplication(sys.argv)
    windo = Main_win()
    windo.show()
    sys.exit(app.exec_())
```

