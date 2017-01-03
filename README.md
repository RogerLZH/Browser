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

###创建浏览器

    