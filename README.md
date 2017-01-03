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
我们可以使用Qt提供的QToolBar创建工具栏

    ...
    # 添加导航栏
    navigation_bar = QToolBar('Navigation')
    # 设定图标的大小
    navigation_bar.setIconSize(QSize(16, 16))
    #添加导航栏到窗口中
    self.addToolBar(navigation_bar)
    ...
QAction类提供了抽象的用户界面action

    #添加按钮
    reload_button = QAction(QIcon('icons/renew.png'), 'reload', self)
将action与实际功能绑定

    reload_button.triggered.connect(self.browser.reload)
这些action可以被放置在窗口部件中

    navigation_bar.addAction(reload_button)
Qt中有一个强大的部件类QWidgets，基于这个类可以派生出很多其他的小部件
，比如QLineEdit是单行文本框，将这个不见作为地址栏，为浏览起添加一个地址栏

    #添加URL地址栏
    self.urlbar = QLineEdit()
Qt中每种组件都有信号机制，可用来将信号与相应的处理函数进行连接绑定，比如将
地址栏的回车信号urlbar.returnPressed与navigate_to_url函数绑定，当地址栏的回车信号发出时
便会触发函数navigate_to_url进行处理

    # 让地址栏能响应回车按键信号
    self.urlbar.returnPressed.connect(self.navigate_to_url)
    #navigate_to_url函数
    def navigate_to_url(self):
        q = QUrl(self.urlbar.text())
        if q.scheme() == '':
            q.setScheme('http')
        self.browser.setUrl(q)

##代码
>项目源码及资源下载： https://github.com/RogerLZH/Browser.git

    # v1.2
    # created
    #   by Roger
    # in 2017.1.3
    
    from PyQt5.QtCore import *
    from PyQt5.QtWidgets import *
    from PyQt5.QtGui import *
    from PyQt5.QtWebKitWidgets import *
    
    import sys
    
    class MainWindow(QMainWindow):
        # noinspection PyUnresolvedReferences
        def __init__(self, *args, **kwargs):
            super().__init__(*args, **kwargs)
            # 设置窗口标题
            self.setWindowTitle('My Browser')
            # 设置窗口图标
            self.setWindowIcon(QIcon('icons/penguin.png'))
            # 设置窗口大小900*600
            self.resize(900, 600)
            self.show()
    
            # 设置浏览器
            self.browser = QWebView()
            url = 'http://blog.csdn.net/roger_lzh'
            # 指定打开界面的 URL
            self.browser.setUrl(QUrl(url))
            # 添加浏览器到窗口中
            self.setCentralWidget(self.browser)
    
    
            ###使用QToolBar创建导航栏，并使用QAction创建按钮
            # 添加导航栏
            navigation_bar = QToolBar('Navigation')
            # 设定图标的大小
            navigation_bar.setIconSize(QSize(16, 16))
            #添加导航栏到窗口中
            self.addToolBar(navigation_bar)
    
            #QAction类提供了抽象的用户界面action，这些action可以被放置在窗口部件中
            # 添加前进、后退、停止加载和刷新的按钮
            back_button = QAction(QIcon('icons/back.png'), 'Back', self)
            next_button = QAction(QIcon('icons/next.png'), 'Forward', self)
            stop_button = QAction(QIcon('icons/cross.png'), 'stop', self)
            reload_button = QAction(QIcon('icons/renew.png'), 'reload', self)
    
            back_button.triggered.connect(self.browser.back)
            next_button.triggered.connect(self.browser.forward)
            stop_button.triggered.connect(self.browser.stop)
            reload_button.triggered.connect(self.browser.reload)
    
            # 将按钮添加到导航栏上
            navigation_bar.addAction(back_button)
            navigation_bar.addAction(next_button)
            navigation_bar.addAction(stop_button)
            navigation_bar.addAction(reload_button)
    
            #添加URL地址栏
            self.urlbar = QLineEdit()
            # 让地址栏能响应回车按键信号
            self.urlbar.returnPressed.connect(self.navigate_to_url)
    
            navigation_bar.addSeparator()
            navigation_bar.addWidget(self.urlbar)
    
            #让浏览器相应url地址的变化
            self.browser.urlChanged.connect(self.renew_urlbar)
    
        def navigate_to_url(self):
            q = QUrl(self.urlbar.text())
            if q.scheme() == '':
                q.setScheme('http')
            self.browser.setUrl(q)
    
        def renew_urlbar(self, q):
            # 将当前网页的链接更新到地址栏
            self.urlbar.setText(q.toString())
            self.urlbar.setCursorPosition(0)
    
    
    # 创建应用
    app = QApplication(sys.argv)
    # 创建主窗口
    window = MainWindow()
    # 显示窗口
    window.show()
    # 运行应用，并监听事件
    app.exec_()    