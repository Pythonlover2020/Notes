# C语言简介
==以下摘自百度百科==
>C语言是一门面向过程的、抽象化的通用程序设计语言，广泛应用于底层开发。C语言能以简易的方式编译、处理低级存储器。C语言是仅产生少量的机器语言以及不需要任何运行环境支持便能运行的高效率程序设计语言。尽管C语言提供了许多低级处理的功能，但仍然保持着跨平台的特性，以一个标准规格写出的C语言程序可在包括类似嵌入式处理器以及超级计算机等作业平台的许多计算机平台上进行编译

# C语言特点
## 优点
* 执行速度特别快（只比汇编和机器码慢）
* 功能强大（几乎万能，但是某些方面比较复杂）
* 编程自由（语法检测不严格）
* 可移植性好（一次编写，到处编译）
## 缺点
* 写代码周期长
* 过于自由，经验不足容易出错
* 对平台库依赖较多
* 安全性差，内存不安全

# C语言环境搭建
## 安装GCC
### 方法一
下载链接：<https://osdn.net/frs/redir.php?m=tuna&f=mingw%2F68260%2Fmingw-get-setup.exe>

![](https://img2020.cnblogs.com/blog/2101031/202008/2101031-20200829190349415-1783535224.png)

![](https://img2020.cnblogs.com/blog/2101031/202008/2101031-20200829190514088-891349958.png)

![](https://img2020.cnblogs.com/blog/2101031/202008/2101031-20200829190542273-1564138173.png)
接着在命令行输入mingw-get install gcc
和mingw-get install g++
### 方法二
安装Qt(<http://m.biancheng.net/qt/>)
MinGW就在D:\Qt\Qt5.9.0\5.9\mingw53_32\bin下，
将此路径添加进环境变量即可

# GCC的使用
>[GCC使用教程](http://m.biancheng.net/gcc/)