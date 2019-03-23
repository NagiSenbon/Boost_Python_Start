# Boost::Python 入坑随笔 （一）

![](https://github.com/NagiSenbon/Boost_Python_Start/raw/master/pic/Amazarashi.jpg)

<!-- TOC -->

- [Boost::Python 入坑随笔 （一）](#boostpython-%E5%85%A5%E5%9D%91%E9%9A%8F%E7%AC%94-%E4%B8%80)
- [前言](#%E5%89%8D%E8%A8%80)
- [环境](#%E7%8E%AF%E5%A2%83)
- [编译 Boost 库](#%E7%BC%96%E8%AF%91-boost-%E5%BA%93)
	- [修改 `user-config.jam` 文件](#%E4%BF%AE%E6%94%B9-user-configjam-%E6%96%87%E4%BB%B6)

<!-- /TOC -->

---

# 前言

[_Boost::Python_](https://wiki.python.org/moin/boost.python) 是 C++ [Boost](https://www.boost.org/) 库中的一个与 Python 交互的模块。[Boost](https://www.boost.org/) 这个库本身有多么神奇就不必多说了，我们还是来谈谈 Boost::Python。
Python 本身就有一个很好的 Python/C API，可以用于 C/C++ 与 Python 的交互。比如用 C++ 编写一个模块，然后 Python 来调用该模块，这种方式能在保证性能的前提下，又不失 Python 的简单方便。而 Boost::Python 就是一个高度封装好的 Python/C API，它能简化 C++ 代码，使得为 Python 编写 C++ 扩展更为简单方便。出于此原因，我也就开始了我的 Boost::Python 入坑之旅。

---

# 环境

先说说我的环境配置:

- Windows 10 64 bit
- [Anaconda Python 3.7.2 ( 64 bit )](https://www.anaconda.com/distribution/)
- [Visual Studio 2017 ( msvc v14.16 )](https://visualstudio.microsoft.com/vs/)
- [Boost 1.69](https://www.boost.org/users/history/version_1_69_0.html)

> 注 : _VS2017_ 和 _Boost 1.69_ 就大家自己去下载吧，链接在上面已经给了。

---

# 编译 Boost 库

编译应该是我踩坑过程当中遇到的最为繁杂的一步了，好在现在已经轻车熟路。不要慌，一步一步来。

## 修改 `user-config.jam` 文件

解压之前下载好了的
