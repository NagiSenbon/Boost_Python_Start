# Boost::Python 入坑随笔 （一）

![](https://github.com/NagiSenbon/Boost_Python_Start/raw/master/pic/Amazarashi.jpg)

<!-- TOC -->

- [Boost::Python 入坑随笔 （一）](#boostpython-%E5%85%A5%E5%9D%91%E9%9A%8F%E7%AC%94-%E4%B8%80)
- [前言](#%E5%89%8D%E8%A8%80)
- [环境](#%E7%8E%AF%E5%A2%83)
- [编译 Boost 库](#%E7%BC%96%E8%AF%91-boost-%E5%BA%93)
	- [修改 `user-config.jam` 文件](#%E4%BF%AE%E6%94%B9-user-configjam-%E6%96%87%E4%BB%B6)
- [引用](#%E5%BC%95%E7%94%A8)

<!-- /TOC -->

---

# 前言

[_Boost::Python_](https://wiki.python.org/moin/boost.python) 是 C++ [Boost](https://www.boost.org/) 库中的一个与 Python 交互的模块。Boost 这个库本身有多么神奇就不必多说了，我们还是来谈谈 Boost::Python。
Python 本身就有一个很好的 [Python / C API](https://docs.python.org/3/c-api/intro.html)，可用于 Python 与 C / C++ 的交互。比如用 C++ 编写一个模块，然后 Python 来调用该模块，这种方式能在保证性能的前提下，又不失 [Pythonic](https://docs.python-guide.org/writing/style/)，何乐而不为呢。

> [Python 之禅](http://note.qidong.name/2018/01/the-zen-of-python/)
>
> **优美胜于丑陋** ( _Python_ 以编写优美的代码为目标 )
> **明了胜于晦涩** ( 优美的代码应当是明了的，命名规范，风格相似 )
> **简洁胜于复杂** ( 优美的代码应当是简洁的，不要有复杂的内部实现 )
> **复杂胜于凌乱** ( 如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁 )
> **...**

而 Boost::Python 就是一个高度封装好的 Python / C API，它能简化 C++ 代码，使得为 Python 编写 C++ 扩展更为简单方便。出于此原因，我也就开始了我的 Boost::Python 入坑之旅。

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

解压之前下载好了的 `boost_1_69_0.zip` ( or `boost_1_69_0.7z` ) 文件。打开解压好的`boost_1_69_0`文件夹，在该文件夹下新建一个`user-config.txt`文件，并在里边加上几句话:

```cpp
using msvc : 14.1;

using python	: 3.7
		: "C:/Users/user name/Anaconda3/python.exe"
		: "C:/Users/user name/Anaconda3/include"
		: "C:/Users/user name/Anaconda3/libs" ;
```


---

# 引用

[Pythonic Code Style](https://docs.python-guide.org/writing/style/)
[Zen of Python (PEP 20)](https://www.python.org/dev/peps/pep-0020/)
