---
layout: post
title:  "GNU radio 安装教程"
date:   2020-05-31 06:53:51 +0800
categories: WEBee
---

# 在Ubuntu18.04下安装GNU radio

### 安装ubuntu18.04
- 下载ubuntu18.04安装包
- 创建虚拟机
- 安装ubuntu18.04
- 更新源

		推荐使用aliyun(open __software & Updates__, select **Download from**,choose **http://mirrors.aliyun.com/ubuntu**)

- 更新库

		sudo apt-get update
		sudo apt-get upgrade

## 从源码安装GNU radio

### 1.安装UHD

- 安装依赖库

		多执行两遍，确保全部安装成功
		sudo apt-get -y install git swig cmake doxygen build-essential libboost-all-dev libtool libusb-1.0-0 libusb-1.0-0-dev libudev-dev libncurses5-dev libfftw3-bin libfftw3-dev libfftw3-doc libcppunit-1.14-0 libcppunit-dev libcppunit-doc ncurses-bin cpufrequtils python-numpy python-numpy-doc python-numpy-dbg python-scipy python-docutils qt4-bin-dbg qt4-default qt4-doc libqt4-dev libqt4-dev-bin python-qt4 python-qt4-dbg python-qt4-dev python-qt4-doc python-qt4-doc libqwt6abi1 libfftw3-bin libfftw3-dev libfftw3-doc ncurses-bin libncurses5 libncurses5-dev libncurses5-dbg libfontconfig1-dev libxrender-dev libpulse-dev swig g++ automake autoconf libtool python-dev libfftw3-dev libcppunit-dev libboost-all-dev libusb-dev libusb-1.0-0-dev fort77 libsdl1.2-dev python-wxgtk3.0 git libqt4-dev python-numpy ccache python-opengl libgsl-dev python-cheetah python-mako python-lxml doxygen qt4-default qt4-dev-tools libusb-1.0-0-dev libqwtplot3d-qt5-dev pyqt4-dev-tools python-qwt5-qt4 cmake git wget libxi-dev gtk2-engines-pixbuf r-base-dev python-tk liborc-0.4-0 liborc-0.4-dev libasound2-dev python-gtk2 libzmq3-dev libzmq5 python-requests python-sphinx libcomedi-dev python-zmq libqwt-dev libqwt6abi1 python-six libgps-dev libgps23 gpsd gpsd-clients python-gps python-setuptools --fix-missing

- 重启系统

		**sudo reboot**

- 准备安装路径

		cd
		mkdir WEBee
		cd WEBee

- 下载源码

		git clone https://gitee.com/helloziyi/uhd.git
		cd uhd

- 切换分支

		git checkout UHD-3.15.LTS

- 准备编译

		cd host
		mkdir build
		cd build
		cmake ../
		make

- 检查编译

		make test

- 安装源码

		sudo make install

- 更新系统共享库缓存

		sudo ldconfig

- 设置环境

		sudo apt-get -y install vim
		sudo vim ~/.bashrc
		如果LD_LIBRARY_PATH未定义，在末尾添加下面的代码
		export LD_LIBRARY_PATH=/usr/local/lib
		如果LD_LIBRARY_PATH已经定义了，在末尾添加下面的代码
		export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib

- 检查安装结果

		uhd_find_devices
		如果看到类似下面的代码，表明安装成功
		linux; GNU C++ version 4.8.4; Boost_105400; UHD_003.010.000.HEAD-0-g6e1ac3fc

		No UHD Devices Found

### 安装GNU radio

- 设置安装路径

		cd ~/WEBee

- 下载源码

		git clone --recursive https://gitee.com/helloziyi/gnuradio.git

- 检出分支,更新子库

		cd gnuradio
		git checkout maint-3.8
		git submodule update --init --recursive

- 安装依赖项

		sudo apt -y install git cmake g++ libboost-all-dev libgmp-dev swig python3-numpy python3-mako python3-sphinx python3-lxml doxygen libfftw3-dev libsdl1.2-dev libgsl-dev libqwt-qt5-dev libqt5opengl5-dev python3-pyqt5 liblog4cpp5-dev libzmq3-dev python3-yaml python3-click python3-click-plugins python3-zmq python3-scipy

- 准备编译

		mkdir build
		cd build
		cmake ../
		在执行make之前，请确保系统有较大的内存（建议3GB以上）
		make

- 检查编译

		make test

- 安装源码

		sudo make install

- 更新系统共享库缓存

		sudo ldconfig

- 检查安装

		gnuradio-config-info --version
		gnuradio-config-info --prefix
		gnuradio-config-info --enabled-components

- 配置环境

		sudo vim ~/.bashrc
		export PYTHONPATH=/usr/local/lib/python3/dist-packages:usr/local/lib/python3/dist-packages:$PYTHONPATH
		export LD_LIBRARY_PATH=usr/local/lib:$LD_LIBRARY_PATH
		sudo ldconfig
		exit
		重新打开终端

- 拨号测试

		python3 ~/WEBee/gnuradio/gr-audio/examples/python/dial_tone.py

- 启动可视化工具

		gnuradio-companion

## 安装802.11和802.15

### 安装gr-foo

	cd ~/WEBee
	git clone https://gitee.com/helloziyi/gr-foo.git
	cd gr-foo
	git checkout maint-3.8
	mkdir build
	cd build
	cmake ..
	make
	make test
	sudo make install
	sudo ldconfig

### 安装gr-ieee802-11

	cd ~/WEBee
	git clone https://gitee.com/helloziyi/gr-ieee802-11.git
	cd gr-ieee802-11
	git checkout maint-3.8
	mkdir build
	cd build
	cmake ..
	make
	sudo make install
	sudo ldconfig

### 安装gr-ieee802-15-4

	cd ~/WEBee
	sudo apt-get -y install python-matplotlib
	git clone https://gitee.com/helloziyi/gr-ieee802-15-4.git
	cd gr-ieee802-15-4
	git checkout maint-3.8
	mkdir build
	cd build
	cmake ..
	make
	sudo make install
	sudo ldconfig

### 调整最大共享内存

	sudo sysctl -w kernel.shmmax=2147483648

### 调整volk最佳实现

	volk_profile

### 下载教程样例

	cd ~/WEBee
	git clone https://gitee.com/helloziyi/gr-tutorial.git

### 开启FTP服务

	sudo apt-get install vsftpd
	sudo vi /etc/vsftpd.conf
		使能如下两行，把注释取消掉
		local_enable=YES
		write_enable=YES
	sudo /etc/init.d/vsftpd restart
	然后通过ftp服务把代码传到ubuntu下

### 安装VScode

	在应用商店搜索VScode并安装
	启动VScode
	安装一下插件
	1)、 C/C++，这个肯定是必须的。
	2)、 C/C++ Snippets，即 C/C++重用代码块。
	3)、 C/C++ Advanced Lint,即 C/C++静态检测 。
	4)、 Code Runner，即代码运行。
	5)、 Include AutoComplete，即自动头文件包含。
	6)、 Rainbow Brackets，彩虹花括号，有助于阅读代码。
	7)、 One Dark Pro， VSCode 的主题。
	8)、 GBKtoUTF8，将 GBK 转换为 UTF8。
	9)、 ARM，即支持 ARM 汇编语法高亮显示。
	10)、 Chinese(Simplified)，即中文环境。
	11)、 vscode-icons， VSCode 图标插件，主要是资源管理器下各个文件夹的图标。
	12)、 compareit，比较插件，可以用于比较两个文件的差异。
	13)、 DeviceTree，设备树语法插件。
	14)、 TabNine，一款 AI 自动补全插件，强烈推荐，谁用谁知道！
	15)、 Python
	16)、 python snippets


## 说明

代码通过gitee转载，可以很快下载源码，UHD使用的版本为3.9，GNU radio使用的版本是3.8

参考网址：

[https://wiki.gnuradio.org/index.php/InstallingGR#From_Source](https://wiki.gnuradio.org/index.php/InstallingGR#From_Source)

[https://www.wime-project.net/installation/](https://www.wime-project.net/installation/)

version：2020/05/30

邮箱：hellocaoziyi@gmail.com
