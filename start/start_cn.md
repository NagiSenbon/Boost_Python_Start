# Boost::Python 入坑随笔 （一）: 编译安装及 Hello World ! (。・∀・)ノ

![](https://github.com/NagiSenbon/Boost_Python_Start/raw/master/pic/Amazarashi.jpg)

<!-- TOC -->

- [Boost::Python 入坑随笔 （一）: 编译安装及 Hello World ! (。・∀・)ノ](#boostpython-%E5%85%A5%E5%9D%91%E9%9A%8F%E7%AC%94-%E4%B8%80-%E7%BC%96%E8%AF%91%E5%AE%89%E8%A3%85%E5%8F%8A-hello-world--%E3%83%BB%E2%88%80%E3%83%BB%E3%83%8E)
	- [前言](#%E5%89%8D%E8%A8%80)
	- [环境](#%E7%8E%AF%E5%A2%83)
	- [编译 Boost 库](#%E7%BC%96%E8%AF%91-boost-%E5%BA%93)
		- [创建 `user-config.jam` 文件](#%E5%88%9B%E5%BB%BA-user-configjam-%E6%96%87%E4%BB%B6)
		- [编译](#%E7%BC%96%E8%AF%91)
			- [b2 / bjam 部分命令参数说明](#b2--bjam-%E9%83%A8%E5%88%86%E5%91%BD%E4%BB%A4%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E)
	- [使用及测试](#%E4%BD%BF%E7%94%A8%E5%8F%8A%E6%B5%8B%E8%AF%95)
		- [创建项目](#%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE)
	- [引用及参考](#%E5%BC%95%E7%94%A8%E5%8F%8A%E5%8F%82%E8%80%83)

<!-- /TOC -->

---

## 前言

[_Boost::Python_](https://wiki.python.org/moin/boost.python) 是 C++ [Boost](https://www.boost.org/) 库中的一个与 Python 交互的模块。Boost 这个库本身有多么神奇就不必多说了，我们还是来谈谈 Boost::Python。
Python 本身就有一个很好的 [Python / C API](https://docs.python.org/3/c-api/intro.html)，可用于 Python 与 C / C++ 的交互。比如用 C++ 编写一个模块，然后 Python 来调用该模块，这种方式能在保证性能的前提下，又不失 [Pythonic](https://docs.python-guide.org/writing/style/)，何乐而不为呢。

> [Python 之禅](http://note.qidong.name/2018/01/the-zen-of-python/)
>
> **优美胜于丑陋** ( _Python_ 以编写优美的代码为目标 )
>
> **明了胜于晦涩** ( 优美的代码应当是明了的，命名规范，风格相似 )
>
> **简洁胜于复杂** ( 优美的代码应当是简洁的，不要有复杂的内部实现 )
>
> **复杂胜于凌乱** ( 如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁 )
>
> **...**

而 Boost::Python 就是一个高度封装好的 Python / C API，它能简化 C++ 代码，使得为 Python 编写 C++ 扩展更为简单方便。出于此原因，我也就开始了我的 Boost::Python 入坑之旅。

---

## 环境

先说说我的环境配置:

- Windows 10 64 bit
- [Anaconda Python 3.7.2 ( 64 bit )](https://www.anaconda.com/distribution/)
- [Visual Studio 2017 ( msvc v14.16 )](https://visualstudio.microsoft.com/vs/)
- [Boost 1.69](https://www.boost.org/users/history/version_1_69_0.html)

注 : _VS 2017_ 和 _Boost 1.69_ 就大家自己去下载吧。

---

## 编译 Boost 库

编译应该是我踩坑过程当中遇到的最为繁杂的一步了，好在现在已经轻车熟路。不要慌，一步一步来。实际上 Boost 较早一些的版本是提供了 Windows 预编译二进制包的，比如 [`Boost 1.67`](https://www.boost.org/users/history/version_1_67_0.html)。但是它里边编译好的是 Python 2.7 的，所以想要自己对应 Python 版本的 Boost::Python，还是得自己编译 (￣ ▽ ￣)"

### 创建 `user-config.jam` 文件

解压之前下载好了的 `boost_1_69_0.zip` ( or `boost_1_69_0.7z` ) 文件。打开解压好的 `boost_1_69_0` 文件夹，在该文件夹下新建一个 `user-config.txt` 文件，并在里边加上几句:

```cpp
using msvc : 14.1;

# 路径仅供参考
using python	: 3.7
		: "C:\Users\user name\Anaconda3\python.exe"
		: "C:\Users\user name\Anaconda3\include"
		: "C:\Users\user name\Anaconda3\libs" ;
```

其中 `using msvc : 14.1` 是指定编译器为 `msvc 14.1` 即 VS 2017，如果是 VS 2015 的话则填 `using msvc : 14.0` 。 `using python` 则是指定 Python 版本及其路径，因为我用的是 Anaconda Python 3.7.2，所以第一处填 3.7，第二处则是 Anaconda 根目录下的 `python.exe` 的目录。第三四处也是在 Anaconda 根目录下，注意顺序和符号填进去就行了。切记版本号和路径填你自己的，别只复制粘贴。保存 `user-config.txt` 文件后并将其后缀修改为 `.jam` ，即 `user-config.jam` 。

### 编译

Boost 库提供了编译工具 `b2.exe` 和 `bjam.exe` ，其中 `b2.exe` 为新版本的编译工具，我们使用它来编译 Boost::Python 64 位静态库。在 `boost_1_69_0` 文件夹下打开命令行 ( Powershell 等)，输入以下命令回车即可。

```shell
.\b2 --with-python --prefix="g:\boost" install toolset=msvc-14.1 link=static address-model=64
```

该指令会同时编译 `Release` 和 `Dubug` 版本，若要单独编译某一个版本，则可使用以下指令:

- Release 版

```shell
.\b2 --with-python --prefix="g:\boost" install toolset=msvc-14.1 variant=release link=static address-model=64
```

- Dubug 版

```shell
.\b2 --with-python --prefix="g:\boost" install toolset=msvc-14.1 variant=debug link=static address-model=64
```

注:由于还要复制各种头文件，所以总耗时大概 10 ~ 20 min。

#### b2 / bjam 部分命令参数说明

- `--with-` | `--without-`
  `--with-` 后面接要编译的 Boost 的库名，如 `--with-python` 即仅编译 Boost::Python 库。相对的，`--without-python` 即为编译除 Boost::Python 之外全部库。如果要编译 ( 或不编译 ) 多个库的话，可用多条 `with | without` 语句来指定，缺省则为全部编译。

- `install` | `stage`
  `stage` 即只生成库，而 `install` 还会生成 `include` 目录。通过 `--prefix="g:\boost"` 指定 `install` 生成好的库的路径为 `"g:\boost"`。如果是 `stage` 则由 `--stagedir=` 来指定。

- `toolset`
  指定编译工具，此处我们指定为 `msvc 14.1`，即 VS 2017 的编译器

- `link`
  即指定编译为动态库还是静态库 ( `.dll | .lib` )，`shared` 即编译为动态库，`static` 即为静态库。我们选择的是编译为静态库。一般而言静态库体积要大一些，但不用带 Boost::Python 的 DLL 文件，部署和使用也较为方便; 动态库的话相对体积较小(也小不了多少)，但必须用带 Boost::Python 的 DLL 文件。选择静态库还是动态库这个得自己取舍。
  ( 小孩子才做选择，我全都要( •̀ ω •́ )✧ )

- `address-model`
  指定编译为 32 位还是 64 位，我们指定 `address-model=64` 即编译为 64 位。

---

## 使用及测试

编译好后怎么能少的了来一个 `Hello World！` 呢？常言道:" 没有经过 `Hello World！` 洗礼的代码是没有灵魂的！"。
走起 ヾ(•ω•`)o

### 创建项目

####

---

## 引用及参考

> [Pythonic Code Style](https://docs.python-guide.org/writing/style/)
>
> [Zen of Python (PEP 20)](https://www.python.org/dev/peps/pep-0020/)
>
> [C++与 Python 混合编程：Boost.python 的安装与使用](https://blog.csdn.net/HaleyPKU/article/details/82911669)
