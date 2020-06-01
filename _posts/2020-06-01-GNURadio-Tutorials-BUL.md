---
layout: post
title:  "GNU radio 安装教程"
date:   2020-05-31 06:53:51 +0800
categories: WEBee
---
# GNU Radio 入门教程

## GNU Radio简介

GNU Radio是一个设计、仿真、部署现实无线电的系统框架，使用流程图搭建系统。GNU Radio可以通过大量现有模块帮助用户快速构建通信系统。

## GNU Radio Companion

GUN Radio的可视化图形界面。要使用该模块，请确保你遵循[GNU Radio安装教程]已经安装好了GNU Radio。使用ctrl+alt+t命令打开终端，输入如下指令开始使用GNU Radio Compaion。

	$ gnuradio-compaion

### 搜索模块

	ctrl+f

### 修改模块属性

	双击模块修改模块属性

### Throttle

	在没有硬件的流程图中，需要使用节流阀【Throttle】来限制采样率，避免死机。
	通常可以是奈奎斯特采样率的3~5倍，使得波形利于观察。

### PMT类型

#### 1.PMT基础

	>>> import pmt
	>>> p = pmt.from_long(23)
	>>> type(p)
	<class 'pmt.pmt_swig.swig_int_ptr'>
	>>> print(p)
	23
	>>> p2 = pmt.from_complex(1j)
	>>> type(p2)
	<class 'pmt.pmt_swig.swig_int_ptr'>
	>>> print(p2)
	0+1i
	>>> pmt.is_complex(p2)
	True

以上是一段python3代码，通过from_long()和from_complex()两个函数来向pmt导入两个不同类型的值，但是二者在pmt中都是相同类型，pmt.pmt_swig.swig_int_ptr。通过这种方式，可以很容易的将数据从python传入到c++中。因为流程图生成代码是python文件，而使用的模块通常是C++编写的，所以使用PMT变量是必要的。对应得C++代码如下：

	#include 
	// [...]
	pmt::pmt_t P = pmt::from_long(23);
	std::cout << P << std::endl;
	pmt::pmt_t P2 = pmt::from_complex(gr_complex(0, 1)); // Alternatively: pmt::from_complex(0, 1)
	std::cout << P2 << std::endl;
	std::cout << pmt::is_complex(P2) << std::endl;

如上所示，通过pmt变量，1.变量能够很容易的打印出来，因为变量都是以字符串的形式存储起来。2.PMT内部记录了这些变量的类型，因此可以使用from\_*的方式导入不同类型的数据，还能通过to_\*的方式将变量转换为其他类型。


	pmt::pmt_t P_int = pmt::from_long(42);
	int i = pmt::to_long(P_int);
	pmt::pmt_t P_double = pmt::from_double(0.2);
	double d = pmt::to_double(P_double);
	pmt::pmt_t P_double = pmt::mp(0.2);

最后一行pmt::mp()函数根据给定变量推断类型，是一种省略的做法。
字符串类型函数比较特别，在pmt中被称作**symbol**，有自己的转换函数

	pmt::pmt_t P_str = pmt::string_to_symbol("spam");
	pmt::pmt_t P_str = pmt::intern("spam"); //string_to_symbol()的别名
	std::string str = pmt::symbol_to_string(P_str);

在[pmt.h](https://www.gnuradio.org/doc/doxygen/pmt_8h.html)头文件中有所有的转换函数。在python中可以利用弱类型，并有一个辅助函数来进行转换。因为python对变量类型并不严格，可以直接使用pmt.to_python()函数。

	P_int = pmt.to_pmt(42)
	i = pmt.to_python(P_int)
	P_double = pmt.to_pmt(0.2)
	d = pmt.to_double(P_double)

还有3个PMT常量

**c++：**

	pmt::pmt_t P_true = pmt::PMT_T;
	pmt::pmt_t P_false = pmt::PMT_F;
	pmt::pmt_t P_nil = pmt::PMT_NIL;

**python：**

	P_true = pmt.PMT_T
	P_false = pmt.PMT_F
	P_nil = pmt.PMT_NIL