#利用Python和PyQt5实现简易浏览器
***
##实验环境
操作系统：Linux Mint  
编辑器：vim  
编程语言：python3
##依赖项安装
安装PyQt5
>Qt是一个跨平台的C++应用程序开发框架

    sudo apt-get install python3-pyqt5
安装完成后进入python命令行界面测试是否安装正确  
    
    python3  
    >>>import PyQt5
执行命令后如果没有任何提示，说明安装成功  
##编程实现
Qt为开发者提供了QtWebKit模块,QtWebKit是一个基于开源项目
WebKit的网页内容渲染引擎，借助该引擎可以更加快捷地将万维
网集成到 Qt 应用中。
>更多参考：http://doc.qt.io/archives/qt-5.5/qtwebkit-index.html

###浏览器有一个可以用于展示网页的窗口
####创建浏览器  
Qt的程序通过创建QApplication类实例来调用exec_()方法进入事件循环，
然后程序一直循环监听各种事件并把它们放入消息队列中，在适当的时候从队
列中取出处理。  

    ...
    #通过创建QApplication类实例来创建应用
    app = QApplication(sys.argv)
    #运行应用并循环监听事件
    app.exec_()

##代码
>项目源码及资源下载： https://github.com/RogerLZH/Browser.git

    # v1.1
    # created
    #   by Roger
    # in 2017.1.3

    from PyQt5.QtCore import *
    from PyQt5.QtWidgets import *
    from PyQt5.QtGui import *
    from PyQt5.QtWebKitWidgets import *
    
    import sys
    
    class MainWindow(QMainWindow):
        #初始化处理函数
        def __init__(self, *args, **kwargs):
            super().__init__(*args, **kwargs)
            # 设置窗口标题
            self.setWindowTitle('My Browser')
            # 设置窗口图标
            self.setWindowIcon(QIcon('icons/penguin.png'))
            #设置窗口大小900*600
            self.resize(900, 600)
            self.show()

            # 设置浏览器
            self.browser = QWebView()
            url = 'http://blog.csdn.net/roger_lzh'
            # 指定打开界面的 URL
            self.browser.setUrl(QUrl(url))
            # 添加浏览器到窗口中
            self.setCentralWidget(self.browser)


    # 创建应用
    app = QApplication(sys.argv)
    # 创建主窗口
    window = MainWindow()
    # 显示窗口
    window.show()
    # 运行应用，并监听事件
    app.exec_()    